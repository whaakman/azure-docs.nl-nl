---
title: Symantec AWS gegevens verbinden met Azure Sentinel Preview | Microsoft Docs
description: Leer hoe u verbinding maken met gegevens van de Symantec AWS Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2019
ms.author: rkarlin
ms.openlocfilehash: 214269bc5c854aa4d3bfd508b0adb5a53ec096df
ms.sourcegitcommit: 80aaf27e3ad2cc4a6599a3b6af0196c6239e6918
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673971"
---
# <a name="connect-azure-sentinel-to-aws"></a>Verbinding maken met Azure Sentinel AWS

De AWS-connector gebruiken om alle gebeurtenissen van uw AWS-CloudTrail in Azure Sentinel streamen. Dit verbindingsproces delegeert toegang voor Azure Sentinel aan uw AWS resource Logboeken, het maken van een vertrouwensrelatie tussen het AWS-CloudTrail en Azure Sentinel. Dit gebeurt op AWS door het maken van een rol met machtigingen tot Azure Sentinel voor toegang tot uw AWS-Logboeken.

## <a name="prerequisites"></a>Vereisten

U moet schrijfmachtiging voor de werkruimte Sentinel van Azure.

## <a name="connect-aws"></a>Verbinding maken met AWS 
 
1.  In de Amazon Web Services-console onder **identiteits- en naleving**, klikt u op **IAM**.

    ![AWS1](./media/connect-aws/aws-1.png)

2.  Kies **rollen** en klikt u op **rol maken**.

    ![AWS2](./media/connect-aws/aws-2.png)

3.  Kies **een andere AWS-account.** In de **Account-ID** en voer de **Microsoft-Account-ID** (**123412341234**) die kan worden gevonden in de AWS-connector-pagina in de portal voor Azure Sentinel.

    ![AWS3](./media/connect-aws/aws-3.png)

4.  Zorg ervoor dat **externe ID vereisen** is geselecteerd en vervolgens en voert u de externe ID (werkruimte-ID), die kan worden gevonden in de pagina voor de AWS-connector in de portal voor Azure Sentinel.

    ![AWS4](./media/connect-aws/aws-4.png)

5.  Under **Attach permissions policy** select **AWSCloudTrailReadOnlyAccess**.

    ![AWS5](./media/connect-aws/aws-5.png)

6.  Geef een Tag (optioneel).

    ![AWS6](./media/connect-aws/aws-6.png)

7.  Voer vervolgens een **rolnaam** en klikt u op de **rol maken** knop.

    ![AWS7](./media/connect-aws/aws-7.png)

8.  Kies in de lijst met rollen, de rol die u hebt gemaakt.

    ![AWS8](./media/connect-aws/aws-8.png)

9.  Kopieer de **rol informatie** en plak deze in de **rol toe te voegen** veld in de Azure-Portal Sentinel.

    ![AWS9](./media/connect-aws/aws-9.png)

10. Zoek voor het gebruik van de relevante schema in Log Analytics voor AWS-gebeurtenissen, **AWSCloudTrail**.



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met AWS-CloudTrail Sentinel van Azure. Zie voor meer informatie over Azure Sentinel, de volgende artikelen:
- Meer informatie over het [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Aan de slag [detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats.md).


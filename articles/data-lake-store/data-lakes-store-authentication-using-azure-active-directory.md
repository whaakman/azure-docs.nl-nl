---
title: Verificatie in Data Lake Store met Azure Active Directory | Microsoft Docs
description: Ontdek hoe u verifieert met Data Lake Store met Azure Active Directory
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 7db48b03a6f34655917c82702c12dbc9bc4c987a
ms.sourcegitcommit: eaad191ede3510f07505b11e2d1bbfbaa7585dbd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39494174"
---
# <a name="authentication-with-data-lake-store-using-azure-active-directory"></a>Verificatie met Data Lake Store met Azure Active Directory

Azure Data Lake Store maakt gebruik van Azure Active Directory voor verificatie. Vóór het ontwerpen van een toepassing die met Azure Data Lake Store werkt, moet u bepalen hoe u aan het verifiëren van uw toepassing met Azure Active Directory (Azure AD).

## <a name="authentication-options"></a>Verificatieopties

* **Verificatie van eindgebruikers** -Azure-referenties van de eindgebruiker worden gebruikt voor verificatie met de Data Lake Store. De toepassing die u maakt om te werken met Data Lake Store vraagt om de referenties voor deze gebruiker. Als gevolg hiervan, deze verificatiemechanisme is *interactieve* en de toepassing wordt uitgevoerd in de context van de aangemelde gebruiker. Zie voor meer informatie en instructies [verificatie van eindgebruikers voor Data Lake Store](data-lake-store-end-user-authenticate-using-active-directory.md).

* **Service-naar-serviceverificatie** -Gebruik deze optie als u wilt dat een toepassing om zichzelf te verifiëren met Data Lake Store. In dergelijke gevallen kunt u een Azure Active Directory (AD)-toepassing maken en de sleutel van de Azure AD-toepassing gebruiken om te verifiëren met Data Lake Store. Als gevolg hiervan, deze verificatiemechanisme is *niet-interactieve*. Zie voor meer informatie en instructies [Service-naar-serviceverificatie voor Data Lake Store](data-lake-store-service-to-service-authenticate-using-active-directory.md).

De volgende tabel ziet u hoe eindgebruikers en service-naar-service-verificatiemechanismen worden ondersteund voor Data Lake Store. Hier ziet u hoe u de tabel lezen.

* Het symbool ✔ * geeft aan dat de optie voor verificatie wordt ondersteund en koppelingen naar een artikel ziet u hoe u de optie voor verificatie. 
* Het symbool ✔ geeft aan dat de verificatieoptie wordt ondersteund. 
* De lege cellen geven aan dat de verificatieoptie niet wordt ondersteund.


|Gebruik deze optie voor verificatie met...                   |.NET         |Java     |PowerShell |CLI 2.0 | Python   |REST     |
|:---------------------------------------------|:------------|:--------|:----------|:-------------|:---------|:--------|
|Eindgebruikers (zonder MFA **)                        |   ✔ |    ✔    |    ✔      |       ✔      |    **[✔ *](data-lake-store-end-user-authenticate-python.md#end-user-authentication-without-multi-factor-authentication)**(afgeschaft)     |    **[✔*](data-lake-store-end-user-authenticate-rest-api.md)**    |
|Eindgebruikers (met MFA)                           |    **[✔*](data-lake-store-end-user-authenticate-net-sdk.md)**        |    **[✔*](data-lake-store-end-user-authenticate-java-sdk.md)**     |    ✔      |       **[✔*](data-lake-store-get-started-cli-2.0.md)**      |    **[✔*](data-lake-store-end-user-authenticate-python.md#end-user-authentication-with-multi-factor-authentication)**     |    ✔    |
|Service-naar-service (met behulp van de sleutel van de client)         |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-client-secret)** |    **[✔*](data-lake-store-service-to-service-authenticate-java.md)**    |    ✔      |       ✔      |    **[✔*](data-lake-store-service-to-service-authenticate-python.md#service-to-service-authentication-with-client-secret-for-account-management)**     |    **[✔*](data-lake-store-service-to-service-authenticate-rest-api.md)**    |
|Service-naar-service (met behulp van clientcertificaat) |    **[✔*](data-lake-store-service-to-service-authenticate-net-sdk.md#service-to-service-authentication-with-certificate)**        |    ✔    |    ✔      |       ✔      |    ✔     |    ✔    |

<i>* Klik op de <b>✔\* </b> symbool. Dit is een koppeling.</i><br>
<i>** MFA staat voor meervoudige verificatie</i>

Zie [Verificatiescenario's voor Azure Active Directory](../active-directory/develop/authentication-scenarios.md) voor meer informatie over het gebruik van Azure Active Directory voor verificatie.

## <a name="next-steps"></a>Volgende stappen

* [Verificatie van de eindgebruiker](data-lake-store-end-user-authenticate-using-active-directory.md)
* [Verificatie van service-tot-service](data-lake-store-service-to-service-authenticate-using-active-directory.md)



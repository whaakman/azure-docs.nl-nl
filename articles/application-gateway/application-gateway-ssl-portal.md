---
title: Configureren van SSL-offload - Azure Application Gateway - Azure-Portal | Microsoft Docs
description: Dit artikel bevat instructies voor het maken van een toepassingsgateway met SSL-offload met behulp van de Azure-portal
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 8373379a-a26a-45d2-aa62-dd282298eff3
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: davidmu
ms.openlocfilehash: 2f7f5d4132e28c8c192d90d5f4bfb2a9034f8b8c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-azure-portal"></a>Een toepassingsgateway voor SSL-offload configureren met behulp van de Azure-portal

> [!div class="op_single_selector"]
> * [Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-ssl-arm.md)
> * [Azure classic PowerShell](application-gateway-ssl.md)
> * [Azure CLI 2.0](application-gateway-ssl-cli.md)

Azure Application Gateway kan zodanig worden geconfigureerd dat de Secure Sockets Layer-sessie (SSL) wordt beëindigd bij de gateway om kostbare SSL-ontsleutelingstaken te voorkomen die worden uitgevoerd in de webfarm. Met SSL-offload worden ook het instellen van de front-endserver en het beheer van de webtoepassing eenvoudiger.

## <a name="scenario"></a>Scenario

Het volgende scenario doorloopt u SSL-offload configureren op een bestaande toepassingsgateway. Het scenario wordt ervan uitgegaan dat u de stappen voor het al hebt gevolgd [een toepassingsgateway maken](application-gateway-create-gateway-portal.md).

## <a name="before-you-begin"></a>Voordat u begint

Er is een certificaat voor SSL-offload met een application gateway configureert, vereist. Dit certificaat is geladen in de toepassingsgateway en wordt gebruikt voor het versleutelen en ontsleutelen van het verkeer dat wordt verzonden via SSL. Het certificaat moet zich in de indeling Personal Information Exchange (.pfx). Deze bestandsindeling kunt u exporteert de persoonlijke sleutel die is vereist voor de toepassingsgateway om uit te voeren voor de versleuteling en ontsleuteling van verkeer.

## <a name="add-an-https-listener"></a>Toevoegen van een HTTPS-listener

De HTTPS-listener wordt gezocht naar verkeer op basis van de configuratie en helpt bij het verkeer naar de back-end-adresgroepen sturen. Als u wilt een HTTPS-listener toevoegen, de volgende stappen uit:

   1. Blader naar de Azure-portal en selecteer een bestaande toepassing.

   2. Selecteer **Listeners**, en selecteer vervolgens de **toevoegen** om toe te voegen een listener.

   ![Gateway overzichtsvenster van de toepassing][1]


   3. Vul de volgende vereiste informatie voor de listener en het pfx-certificaat uploaden:
      - **Naam**: de beschrijvende naam van de listener.

      - **Frontend-IP configuratie**: de front-end-IP-configuratie die wordt gebruikt voor de listener.

      - **Frontend-poort (naam/poort)**: een beschrijvende naam voor de poort die wordt gebruikt op de front-end van de toepassingsgateway en de werkelijke poort die wordt gebruikt.

      - **Protocol**: een schakeloptie om te bepalen als HTTPS of HTTP voor de front-endserver gebruikt.

      - **Certificaat (naam en wachtwoord)**: als SSL-offload wordt gebruikt, is een pfx-certificaat vereist voor deze instelling. Een beschrijvende naam en wachtwoord zijn ook vereist.

   4. Selecteer **OK**.

![Een listener-deelvenster toevoegen][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>Een regel maken en deze koppelen aan de listener

De listener is gemaakt. Maak vervolgens een regel voor het afhandelen van het verkeer van de listener. Regels definiëren hoe verkeer wordt doorgestuurd naar de back-end-adresgroepen op basis van meerdere configuratie-instellingen. Deze instellingen omvatten het protocol, de poort en de statuscontroles, en of de sessie cookies gebaseerde affiniteit gebruik is. Maken en koppelen van een regel aan de listener, als volgt te werk:


   1. Selecteer de **regels** van de toepassingsgateway, en selecteer vervolgens **toevoegen**.

   ![Deelvenster van de Gateway regels toepassing][3]


   2. Onder **basic regel toevoegen**, een beschrijvende naam voor de regel in de **naam** veld en selecteer vervolgens de **Listener** in de vorige stap hebt gemaakt. Selecteer de relevante **back-endpool** en **HTTP-instelling**, en selecteer vervolgens **OK**.

   ![Venster Instellingen voor HTTPS][4]

De instellingen zijn nu opgeslagen op de toepassingsgateway. Het opslaan verwerken voor deze instellingen kunnen even duren voordat ze beschikbaar zijn voor weergeven via de portal of PowerShell. Nadat deze is opgeslagen, verwerkt de toepassingsgateway te coderen en decoderen van verkeer. Al het verkeer tussen de toepassingsgateway en de back-end-webservers wordt verwerkt via HTTP. Alle communicatie naar de client wordt als gestart via HTTPS, geretourneerd naar de client die is versleuteld.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het configureren van een aangepaste health test met Azure Application Gateway, [maken van een aangepaste health test](application-gateway-create-gateway-portal.md).

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png


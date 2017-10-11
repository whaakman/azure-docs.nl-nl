---
title: Het configureren van een toepassing toepassingsproxy | Microsoft Docs
description: Informatie over het maken van een configureren een toepassingsproxy-toepassing in een paar eenvoudige stappen
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c8f98536048a85ebb3f061d840457130579196d9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-an-application-proxy-application"></a>Het configureren van een toepassing toepassingsproxy

In dit artikel helpt u bij het begrijpen van het configureren van een toepassing toepassingsproxy in Azure AD om uw on-premises toepassingen naar de cloud weer te geven.

## <a name="recommended-documents"></a>Aanbevolen documenten 

Voor meer informatie over de eerste configuraties en het maken van een toepassing toepassingsproxy via de beheerportal, volgt u de [toepassingen publiceren met Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Zie voor meer informatie over het configureren van Connectors [toepassingsproxy inschakelen in de Azure portal](active-directory-application-proxy-enable.md).

Zie voor meer informatie over het uploaden van certificaten en gebruiken van aangepaste domeinen [werken met aangepaste domeinen in Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).

## <a name="create-the-applicationsetting-the-urls"></a>De URL's voor de toepassingsinstelling maken

Als u volgt de stappen in de [toepassingen publiceren met Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) documentatie en zijn voor het ophalen van een fout bij het maken van de toepassing, Zie de foutdetails voor informatie en suggesties voor het oplossen van de toepassing. De meeste foutberichten bevatten een voorgestelde oplossing. Controleer of algemene fouten te voorkomen:

-   U bent een beheerder met een machtiging voor het maken van een toepassing toepassingsproxy

-   De interne URL is uniek.

-   De externe URL is uniek.

-   De URL's met http of https beginnen en eindigen met een "/"

-   De URL moet een domeinnaam, geen IP-adres

Het foutbericht moet worden weergegeven in de rechterbovenhoek bij het maken van de toepassing. U kunt ook het meldingspictogram voor de foutberichten selecteren.

   ![Prompt voor melding](./media/application-proxy-config-how-to/error-message.png)

## <a name="configure-connectorsconnector-groups"></a>Connectors/connector groepen configureren

Als u problemen bij het configureren van uw toepassing vanwege een waarschuwing over connectors en connector groepen ondervindt, raadpleegt u de instructies op de toepassingsproxy inschakelen voor meer informatie over het downloaden van connectors. Als u weten over connectors wilt, raadpleegt u de [connectors documentatie](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).

Als uw connectors niet actief zijn, betekent dit dat ze niet bereiken van de service zijn. Dit is vaak omdat niet de vereiste poorten geopend zijn. Een lijst met vereiste poorten, Zie de sectie vereisten van de documentatie van de toepassingsproxy inschakelen.

## <a name="upload-certificates-for-custom-domains"></a>Certificaten voor aangepaste domeinen uploaden

Aangepaste domeinen kunnen u het domein van de externe URL's opgeven. Voor het gebruik van aangepaste domeinen, moet u het certificaat voor dat domein uploaden. Zie voor meer informatie over het gebruik van aangepaste domeinen en certificaten [werken met aangepaste domeinen in Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains). 

Als er problemen zijn uw certificaat uploaden, zoekt u de foutberichten in de portal voor meer informatie over het probleem met het certificaat. Algemene problemen met de certificaten zijn onder andere:

-   Verlopen certificaat

-   Certificaat is zelfondertekend

-   Certificaat ontbreekt voor de persoonlijke sleutel

Het foutbericht wordt weergegeven in de rechterbovenhoek als u probeert om het certificaat te uploaden. U kunt ook het meldingspictogram voor de foutberichten selecteren.

   ![Prompt voor melding](./media/application-proxy-config-how-to/error-message2.png)

## <a name="next-steps"></a>Volgende stappen
[Toepassingen publiceren met Azure AD-toepassingsproxy](application-proxy-publish-azure-portal.md)

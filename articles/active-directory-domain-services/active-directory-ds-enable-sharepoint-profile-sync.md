---
title: 'Azure Active Directory Domain Services: Ondersteuning voor SharePoint gebruikersprofielservice inschakelen | Microsoft Docs'
description: Azure Active Directory Domain Services beheerde domeinen voor profielsynchronisatie voor SharePoint-Server configureren
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: ergreenl
ms.openlocfilehash: 982984912494598e9791968e5ee3ccee52c5a7c9
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50154918"
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>Een beheerd domein ter ondersteuning van profielsynchronisatie voor SharePoint-Server configureren
SharePoint-Server bevat een gebruiker profiel-Service die wordt gebruikt voor synchronisatie van het profiel. Om in te stellen de gebruikersprofielservice, moeten de juiste machtigingen om te worden toegekend op Active Directory-domein. Zie voor meer informatie, [Active Directory Domain Services machtigingen verlenen voor profielsynchronisatie in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).

In dit artikel wordt uitgelegd hoe u Azure AD Domain Services beheerde domeinen voor het implementeren van de synchronisatieservice van SharePoint Server gebruiker profiel kunt configureren.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="the-aad-dc-service-accounts-group"></a>De groep AAD DC serviceaccounts
Een beveiligingsgroep met de naam '**AAD DC-serviceaccounts**' is beschikbaar in de organisatie-eenheid 'Gebruikers' op uw beheerde domein. U ziet deze groep in de **Active Directory: gebruikers en Computers** MMC-module op uw beheerde domein.

![DC-serviceaccounts AAD-beveiligingsgroep](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Leden van deze beveiligingsgroep zijn overgedragen van de volgende bevoegdheden:
- De bevoegdheid 'Directorywijzigingen' op de hoofd-DSE van het beheerde domein.
- De bevoegdheid 'Directorywijzigingen' op de naamgevingscontext configuratie (cn = configuratiecontainer) van het beheerde domein.

Deze beveiligingsgroep is ook lid is van de ingebouwde groep **Pre-Windows 2000-compatibele toegang**.

![DC-serviceaccounts AAD-beveiligingsgroep](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>Uw beheerde domein voor de ondersteuning van synchronisatie van gebruikersprofielen SharePoint-Server inschakelen
U kunt het serviceaccount gebruikt voor SharePoint gebruiker profielsynchronisatie met toevoegen de **AAD DC-serviceaccounts** groep. Als gevolg hiervan wordt het account voor synchronisatie voldoende bevoegdheden voor het repliceren van wijzigingen naar de map. Deze configuratiestap kunt synchronisatie van gebruikersprofielen SharePoint-Server correct te laten werken.

![Service-Accounts van AAD DC - toevoegen leden](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![Service-Accounts van AAD DC - toevoegen leden](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Gerelateerde inhoud
* [Technische documentatie - machtigingen verlenen Active Directory Domain Services voor profielsynchronisatie in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx)

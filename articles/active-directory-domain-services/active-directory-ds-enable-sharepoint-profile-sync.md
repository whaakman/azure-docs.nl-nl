---
title: 'Azure Active Directory Domain Services: Ondersteuning voor het gebruikersprofiel SharePoint-service inschakelen | Microsoft Docs'
description: Azure Active Directory Domain Services beheerde domeinen ter ondersteuning van synchronisatie van het profiel voor SharePoint-Server configureren
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: c3c923b5c9cd652f0c5b0ec98c1cda740f180122
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>Een beheerd domein ter ondersteuning van synchronisatie van het profiel voor SharePoint-Server configureren
SharePoint Server bevat een User Profile-Service die wordt gebruikt voor synchronisatie van het profiel. Als u de Service-profiel instelt, moeten geschikte machtigingen worden toegekend op Active Directory-domein. Zie voor meer informatie [Active Directory Domain Services-machtigingen voor synchronisatie van het profiel in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).

Dit artikel wordt uitgelegd hoe u Azure AD Domain Services beheerde domeinen voor het implementeren van de service voor SharePoint serversynchronisatie van gebruikersprofielen kunt configureren.

## <a name="the-aad-dc-service-accounts-group"></a>De groep AAD DC serviceaccounts
Een beveiligingsgroep genaamd '**AAD DC-serviceaccounts**' is beschikbaar in de organisatie-eenheid 'Gebruikers' in uw beheerde domein. Ziet u deze groep in de **Active Directory: gebruikers en Computers** MMC-module op uw beheerde domein.

![DC-serviceaccounts AAD-beveiligingsgroep](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

Leden van deze beveiligingsgroep gedelegeerde de volgende bevoegdheden:
- De bevoegdheid 'directorywijzigingen repliceren' in de hoofdmap DSE van het beheerde domein.
- De bevoegdheid 'directorywijzigingen repliceren' in de naamgevingscontext configuratie (cn = configuratiecontainer) van het beheerde domein.

Deze beveiligingsgroep is ook lid zijn van de ingebouwde groep **Pre-Windows 2000-compatibele toegang**.

![DC-serviceaccounts AAD-beveiligingsgroep](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>Uw beheerde domein voor de ondersteuning van synchronisatie van gebruikersprofielen SharePoint-Server inschakelen
U kunt het serviceaccount dat wordt gebruikt voor SharePoint gebruiker profielsynchronisatie toevoegen de **AAD DC-serviceaccounts** groep. De Synchronisatieaccount opgehaald als gevolg hiervan onvoldoende machtigingen om wijzigingen te repliceren naar de map. Deze stap in de configuratie kunt synchronisatie van gebruikersprofielen SharePoint-Server correct te laten werken.

![Serviceaccounts AAD DC - toevoegen leden](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![Serviceaccounts AAD DC - toevoegen leden](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Gerelateerde inhoud
* [Technische documentatie - machtigingen verlenen Active Directory Domain Services voor synchronisatie van het profiel in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx)

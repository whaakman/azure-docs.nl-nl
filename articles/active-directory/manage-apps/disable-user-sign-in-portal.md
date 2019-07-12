---
title: Gebruikersaanmeldingen voor een enterprise-app in Azure Active Directory uitschakelen | Microsoft Docs
description: Het uitschakelen van een bedrijfstoepassing, zodat er geen gebruikers me niet bij Azure Active Directory aanmelden kunnen
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/12/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7256791c0b6bfbc72a26f6093cdd3c39410f702f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807594"
---
# <a name="disable-user-sign-ins-for-an-enterprise-app-in-azure-active-directory"></a>Gebruikersaanmeldingen voor een enterprise-app in Azure Active Directory uitschakelen

Het is eenvoudig om uit te schakelen van een bedrijfstoepassing, zodat er geen gebruikers bij het in Azure Active Directory (Azure AD aanmelden zich). U moet de juiste machtigingen voor het beheren van de enterprise-app. En u moet globale beheerder voor de map.

## <a name="how-do-i-disable-user-sign-ins"></a>Hoe kan ik gebruikersaanmeldingen uitschakelen?

1. Meld u aan bij [Azure Portal](https://portal.azure.com) met een account van een globale beheerder voor de directory.
1. Selecteer **alle services**, voer **Azure Active Directory** in het tekstvak in en selecteer vervolgens **Enter**.
1. Op de **Azure Active Directory** -  ***directoryname*** deelvenster (dat wil zeggen, de Azure AD voor de map die u beheert), selecteer **bedrijfstoepassingen**.
1. Op de **bedrijfstoepassingen - alle toepassingen** deelvenster ziet u een lijst van de apps die u kunt beheren. Selecteer een app.
1. Op de ***appname*** deelvenster (dat wil zeggen, het deelvenster met de naam van de geselecteerde app in de titel), selecteer **eigenschappen**.
1. Op de ***appname*** - **eigenschappen** venster **Nee** voor **ingeschakeld voor gebruikers om aan te melden?** .
1. Selecteer de **opslaan** opdracht.

## <a name="next-steps"></a>Volgende stappen

* [Mijn groepen bekijken](../fundamentals/active-directory-groups-view-azure-portal.md)
* [Een gebruiker of groep toewijzen aan een enterprise-app](assign-user-or-group-access-portal.md)
* [De toewijzing van een gebruiker of groep verwijderen uit een enterprise-app](remove-user-or-group-access-portal.md)
* [De naam of het logo van een zakelijke app wijzigen](change-name-or-logo-portal.md)

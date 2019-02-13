---
title: Om weer te geven van de service-principal van een beheerde identiteit in Azure portal
description: Stapsgewijze instructies voor het weergeven van de service-principal van een beheerde identiteit in Azure portal.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2018
ms.author: priyamo
ms.collection: M365-identity-device-management
ms.openlocfilehash: d6d2896b43d9eb5e126d07353a4bf6fedfd11542
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56183867"
---
# <a name="view-the-service-principal-of-a-managed-identity-in-the-azure-portal"></a>De service-principal van een beheerde identiteit in Azure portal bekijken

Beheerde identiteiten voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory. U kunt deze identiteit gebruiken om te verifiëren bij een service die ondersteuning biedt voor Azure AD-verificatie, zonder referenties in uw code. 

In dit artikel leert u hoe u om de service-principal van een beheerde identiteit met Azure portal weer te geven.

## <a name="prerequisites"></a>Vereisten

- Als u niet bekend met beheerde identiteiten voor Azure-resources bent, lees de [overzichtssectie](overview.md).
- Als u nog een Azure-account hebt [zich registreren voor een gratis account](https://azure.microsoft.com/free/).
- Schakel [systeem toegewezen identiteit op een virtuele machine](/azure/active-directory/managed-identities-azure-resources/qs-configure-portal-windows-vm#system-assigned-managed-identity) of [toepassing](/azure/app-service/overview-managed-identity#adding-a-system-assigned-identity).

## <a name="view-the-service-principal"></a>De service-principal weergeven

Deze procedure ziet u hoe u de service-principal van een virtuele machine weergeven met systeem toegewezen identiteit is ingeschakeld (de dezelfde stappen gelden voor een toepassing).

1. Klik op **Azure Active Directory** en klik vervolgens op **bedrijfstoepassingen**.
2. Onder **toepassingstype**, kiest u **alle toepassingen**.
3. In het zoekvak filter, typ de naam van de virtuele machine of toepassing die de beheerde identiteit is ingeschakeld of kiezen uit de lijst weergegeven.

   ![Weergave beheerde identiteit service-principal in portal](./media/how-to-view-managed-identity-service-principal-portal/view-managed-identity-service-principal-portal.png)

## <a name="next-steps"></a>Volgende stappen

[Beheerde identiteiten voor Azure-resources](/azure/active-directory/managed-identities-azure-resources/overview)


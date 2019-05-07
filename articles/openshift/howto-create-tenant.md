---
title: Maken van een Azure AD-tenant voor Azure Red Hat OpenShift | Microsoft Docs
description: Hier is het maken van een tenant Azure Active Directory (Azure AD) voor het hosten van uw Microsoft Azure Red Hat OpenShift-cluster.
author: tylermsft
ms.author: twhitney
ms.service: container-service
manager: jeconnoc
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 3c7f0b263cc775b7f9fec62e4d5388250e5124fb
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079118"
---
# <a name="create-an-azure-ad-tenant-for-azure-red-hat-openshift"></a>Maken van een Azure AD-tenant voor Azure Red Hat OpenShift

Microsoft Azure Red Hat OpenShift vereist een [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) tenant in te maken van uw cluster. Een *tenant* is een toegewezen exemplaar van Azure AD dat een organisatie of app-ontwikkelaar ontvangt wanneer ze een relatie met Microsoft maken door de registratie voor Azure, Microsoft Intune of Microsoft 365. Elke Azure AD-tenant is uniek en die is gescheiden van andere Azure AD-tenants en heeft een eigen werk en school-id's en app-registraties.

Als u nog een Azure AD-tenant hebt, volgt u deze instructies voor het maken van een.

## <a name="create-a-new-azure-ad-tenant"></a>Een nieuwe Azure AD-tenant maken

Een tenant maken:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com/) met behulp van het account dat u wilt koppelen aan uw Azure Red Hat OpenShift-cluster.
2. Open de [Azure Active Directory-blade](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory) naar een nieuwe tenant maken (ook wel bekend als een nieuwe *Azure Active Directory*).
3. Geef een **organisatienaam**.
4. Geef een **initiële domeinnaam**. Dit is *onmicrosoft.com* hieraan toegevoegd. U kunt de waarde voor hergebruiken *organisatienaam* hier.
5. Kies een land of regio waar de tenant wordt gemaakt.
6. Klik op **Create**.
7. Nadat uw Azure AD-tenant is gemaakt, selecteert u de **Klik hier voor het beheren van uw nieuwe adreslijst** koppeling. De naam van uw nieuwe tenant moet worden weergegeven in de rechterbovenhoek van de Azure-portal:  

    ![Schermafbeelding van de portal geeft de naam van de tenant in de rechterbovenhoek][tenantcallout]  

8. Noteer de *tenant-ID* , zodat u kunt later opgeven waar u om uw Azure Red Hat OpenShift-cluster te maken. In de portal ziet u nu de overzichtsblade van Azure Active Directory voor de nieuwe tenant. Selecteer **eigenschappen** en kopieer de waarde voor uw **map-ID**. Er wordt verwezen naar deze waarde als de `tenant id` in de [maken van een cluster Azure Red Hat OpenShift](tutorial-create-cluster.md) zelfstudie.

[tenantcallout]: ./media/howto-create-tenant/tenant-callout.png

## <a name="resources"></a>Resources

Bekijk [Azure Active Directory-documentatie](https://docs.microsoft.com/azure/active-directory/) voor meer informatie over [Azure AD-tenants](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).

## <a name="next-steps"></a>Volgende stappen

Informatie over het maken van een service-principal, een client-geheim en de verificatie callback-URL te genereren en maak een nieuwe Active Directory-gebruiker voor het testen van apps op uw cluster Azure Red Hat OpenShift.

[Een object voor Azure AD-app en gebruiker maken](howto-aad-app-configuration.md)
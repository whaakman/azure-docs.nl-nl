---
title: Een Azure Management API-certificaat uploaden | Microsoft Docs
description: Informatie over het uploaden van athe API Management-certificaat voor de klassieke Azure Portal.
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: adegeo
ms.openlocfilehash: 89c08f2d1e0e244503e3fc4355cad0b4391618e1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="upload-an-azure-management-api-management-certificate"></a>Een Azure Management API Management-certificaat uploaden
Van beheercertificaten kunnen u verifiëren met het klassieke implementatiemodel verstrekt door Azure. Veel programma's en hulpprogramma's (zoals Visual Studio of de Azure SDK) gebruik van deze certificaten in configuratie en implementatie van verschillende Azure-services te automatiseren. 

> [!WARNING]
> Wees voorzichtig! Deze typen certificaten dat alle gebruikers worden geverifieerd met hen voor het beheren van het abonnement dat ze zijn gekoppeld.
>
>

Als u meer informatie over Azure-certificaten (zoals het maken een zelfondertekend certificaat), Zie [certificaten voor Azure Cloud Services-overzicht](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

U kunt ook [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) om te verifiëren clientcode voor automatisering doeleinden.

**Opmerking:** moet u medebeheerder voor het abonnement onder Beheercertificaten bewerkingen uitvoeren. [Meer informatie](https://go.microsoft.com/fwlink/?linkid=849300) over het toevoegen of verwijderen van Co-beheerders van nieuwe Azure-Portal 

## <a name="upload-a-management-certificate"></a>Een management-certificaat uploaden
Zodra u hebt een beheercertificaat dat is gemaakt, (CER-bestand met alleen de openbare sleutel) kunt u deze in de portal uploaden. Wanneer het certificaat in de portal beschikbaar is, kan iedereen met een overeenkomend certificaat (persoonlijke sleutel) verbinding maken via de API Management en toegang tot de bronnen voor het gekoppelde abonnement.

1. Meld u aan bij [Azure Portal](http://portal.azure.com).
2. Klik op **meer services** op de lijst van de Azure-service onder Selecteer **abonnementen** in de _algemene_ servicegroep.

    ![Abonnement-menu](./media/azure-api-management-certs/subscriptions_menu.png)

3. Zorg ervoor dat het juiste abonnement die u wilt koppelen aan het certificaat te selecteren.     
4. Nadat u het juiste abonnement hebt geselecteerd, drukt u op **beheercertificaten** in de _instellingen_ groep.

    ![Instellingen](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Druk op de **uploaden** knop.

    ![Op de pagina certificaten uploaden](./media/azure-api-management-certs/certificates_page.png)
6. Vul het dialoogvenster informatie en druk op **uploaden**.

    ![Instellingen](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Volgende stappen
Nu u een beheercertificaat dat is gekoppeld aan een abonnement hebt, kunt u (nadat u de overeenkomende certificaat lokaal hebt geïnstalleerd) programmatisch koppelen aan de [klassieke implementatiemodel REST-API](https://msdn.microsoft.com/library/azure/mt420159.aspx) en het automatiseren van de verschillende Azure-resources die ook gekoppeld aan dat abonnement.

---
title: Een Azure Service Management-certificaat uploaden | Microsoft Docs
description: Informatie over het uploaden van het Service Management-certificaat voor de Azure-portal.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: api-management
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeconnoc
ms.openlocfilehash: 014a26c2500959502eeb1c50d3f311584c1ad84e
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895682"
---
# <a name="upload-an-azure-service-management-certificate"></a>Een Azure Service Management-certificaat uploaden
Van beheercertificaten kunnen u verifiëren met het klassieke implementatiemodel verstrekt door Azure. Veel programma's en hulpprogramma's (zoals Visual Studio of de Azure SDK) kunt u deze certificaten gebruiken voor het automatiseren van configuratie en implementatie van verschillende Azure-services. 

> [!WARNING]
> Wees voorzichtig! Deze typen certificaten toestaan iedereen die met hen verifieert voor het beheren van het abonnement dat ze zijn gekoppeld.
>
>

Als u graag meer informatie over Azure-certificaten (inclusief het maken van een zelfondertekend certificaat), raadpleegt u [overzicht van certificaten voor Azure Cloud Services](cloud-services/cloud-services-certs-create.md#what-are-management-certificates).

U kunt ook [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) om te verifiëren clientcode voor automation-doeleinden.

**Opmerking:** U moet een CO-beheerder van het abonnement bewerkingen onder beheer van certificaten uit te voeren. [Meer informatie](https://go.microsoft.com/fwlink/?linkid=849300) over het toevoegen of verwijderen van Co-beheerders van de nieuwe Azure Portal 

## <a name="upload-a-management-certificate"></a>Een beheercertificaat te uploaden
Zodra u hebt een beheercertificaat gemaakt, (CER-bestand met alleen de openbare sleutel) kunt u deze kunt uploaden in de portal. Wanneer het certificaat in de portal beschikbaar is, kan iedereen met een overeenkomend certificaat (persoonlijke sleutel) verbinding maken via de API voor beheer en toegang tot de bronnen voor het gekoppelde abonnement.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik op **alle services** in de lijst van de Azure-service onder Selecteer **abonnementen** in de _algemene_ -servicegroep.

    ![Menu voor abonnement](./media/azure-api-management-certs/subscriptions_menu.png)

3. Zorg ervoor dat u het juiste abonnement die u wilt koppelen aan het certificaat te selecteren.     
4. Nadat u het juiste abonnement hebt geselecteerd, drukt u op **beheercertificaten** in de _instellingen_ groep.

    ![Instellingen](./media/azure-api-management-certs/mgmtcerts_menu.png)

5. Druk op de **uploaden** knop.

    ![Op de pagina certificaten uploaden](./media/azure-api-management-certs/certificates_page.png)
6. Vul de informatie over en druk op **uploaden**.

    ![Instellingen](./media/azure-api-management-certs/certificate_details.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u een beheercertificaat dat is gekoppeld aan een abonnement hebt, u kunt (nadat u de overeenkomende certificaat lokaal hebt geïnstalleerd) via een programma verbinding maken met de [klassieke implementatiemodel REST-API](/azure/#pivot=sdkstools) en automatiseren van de verschillende Azure-resources die ook gekoppeld aan dat abonnement zijn.

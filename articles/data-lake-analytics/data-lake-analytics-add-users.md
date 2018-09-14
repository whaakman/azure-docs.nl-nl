---
title: Gebruikers toevoegen aan een Azure Data Lake Analytics-account
description: Meer informatie over het correct gebruikers toevoegen aan uw Data Lake Analytics-account
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: conceptual
ms.date: 05/24/2018
ms.openlocfilehash: f48dc07e27c6cb01a842f1f6d720ed6476028ef7
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542309"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Een gebruiker toe te voegen in de Azure-portal

## <a name="start-the-add-user-wizard"></a>Start de Wizard gebruiker toevoegen
1. Open uw Azure Data Lake Analytics via https://portal.azure.com.
2. Klik op **Wizard gebruiker toevoegen**.
3. In de **gebruiker selecteren** stap, zoek de gebruiker die u wilt toevoegen. Klik op **Selecteren**.
4. de **rol selecteren** stap, kies **Data Lake Analytics-ontwikkelaar**. Deze rol heeft het minimale aantal vereiste machtigingen voor het U-SQL-taken indienen/controleren/beheren. Toewijzen aan deze rol als de groep is niet bedoeld voor het beheren van Azure-services.
5. In de **catalogusmachtigingen selecteren** stap, selecteert u eventuele andere databases die gebruiker toegang nodig tot. Lees- en schrijftoegang tot de database master is vereist voor het verzenden van taken. Klik als u klaar bent op **OK**.
6. In de laatste stap met de naam **geselecteerde machtigingen toewijzen** Controleer de wijzigingen die de wizard maakt. Klik op **OK**.


## <a name="configure-acls-for-data-folders"></a>Toegangsbeheerlijsten voor mappen configureren
Verlenen van 'L' of "LSU", indien nodig, op mappen met invoergegevens- en uitvoergegevens.


## <a name="optionally-add-the-user-to-the-azure-data-lake-storage-gen1-role-reader-role"></a>(Optioneel) de gebruiker toevoegen aan de rol van Azure Data Lake Storage Gen1 **lezer** rol.
1.  De account van uw Azure Data Lake Storage Gen1 vinden.
2.  Klik op **gebruikers**.
3. Klik op **Add**.
4.  Selecteer een Azure RBAC-rol aan deze groep worden toegewezen.
5.  Toewijzen aan de rol van lezer. Deze rol heeft het minimale aantal vereiste machtigingen voor gegevens die zijn opgeslagen in ADLSGen1 Bladeren/beheren. Toewijzen aan deze rol als de groep is niet bedoeld voor het beheren van Azure-services.
6.  Typ de naam van de groep.
7.  Klik op **OK**.

## <a name="adding-a-user-using-powershell"></a>Toevoegen van een gebruiker met behulp van PowerShell

1. Volg de instructies in deze handleiding: [hoe u Azure PowerShell installeren en configureren](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Download de [toevoegen AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1) PowerShell-script.
3. Voer het PowerShell-script. 

De voorbeeldopdracht te geven van de gebruikerstoegang tot het verzenden van taken, nieuwe taak metagegevens en weergave oude metagegevens weergeven:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Data Lake Analytics met behulp van Azure portal](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics beheren met behulp van Azure PowerShell](data-lake-analytics-manage-use-powershell.md)


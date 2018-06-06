---
title: Gebruikers toevoegen aan een Azure Data Lake Analytics-account
description: Meer informatie over het correct gebruikers toevoegen aan uw Data Lake Analytics-account
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
manager: kfile
editor: jasonwhowell
ms.assetid: db35f16e-1565-4873-a851-bd987accdc58
ms.topic: conceptual
ms.date: 05/24/2018
ms.openlocfilehash: 58b78c7d9769069f36c9f01c2a7650878a6c5ec9
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34717228"
---
# <a name="adding-a-user-in-the-azure-portal"></a>Een gebruiker toe te voegen in de Azure portal

## <a name="start-the-add-user-wizard"></a>Start de Wizard gebruiker toevoegen
1. Open uw Azure Data Lake Analytics via https://portal.azure.com.
2. Klik op **Wizard gebruiker toevoegen**.
3. In de **gebruiker selecteren** stap, zoek de gebruiker die u wilt toevoegen. Klik op **Selecteren**.
4. de **Functieservices selecteren** stap, kies **Data Lake Analytics Developer**. Deze rol is de minimale set van de vereiste machtigingen voor de U-SQL-taken verzenden of monitor/beheren. Toewijzen aan deze rol als de groep is niet bedoeld voor het beheren van Azure-services.
5. In de **catalogus machtigingen selecteren** stap, selecteert u een extra databases die gebruiker toegang nodig tot. Lees- en schrijftoegang tot de database master is vereist voor het verzenden van taken. Klik als u klaar bent op **OK**.
6. In de laatste stap aangeroepen **geselecteerde machtigingen toewijzen aan** Controleer de wijzigingen die de wizard brengt. Klik op **OK**.


## <a name="configure-acls-for-data-folders"></a>ACL's configureren voor de gegevensmappen
'R-X' of 'RWX' verlenen, indien nodig, op mappen met invoergegevens- en uitvoergegevens.


## <a name="optionally-add-the-user-to-the-azure-data-lake-store-role-reader-role"></a>Voeg desgewenst de gebruiker aan de Azure Data Lake Store-functie **lezer** rol.
1.  Uw Azure Data Lake Store-account vinden.
2.  Klik op **gebruikers**.
3. Klik op **Add**.
4.  Selecteer een Azure RBAC-rol voor deze groep worden toegewezen.
5.  Toewijzen aan de rol lezer. Deze rol is de minimale set van de vereiste machtigingen voor gegevens die zijn opgeslagen in ADLS bladeren of beheren. Toewijzen aan deze rol als de groep is niet bedoeld voor het beheren van Azure-services.
6.  Typ de naam van de groep.
7.  Klik op **OK**.

## <a name="adding-a-user-using-powershell"></a>Een gebruiker met behulp van PowerShell toe te voegen

1. Volg de instructies in deze handleiding: [installeren en configureren van Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
2. Download de [toevoegen AdlaJobUser.ps1](https://github.com/Azure/AzureDataLake/blob/master/Samples/PowerShell/ADLAUsers/Add-AdlaJobUser.ps1) PowerShell-script.
3. Het PowerShell-script uitvoeren. 

De voorbeeldopdracht geven de gebruikerstoegang tot het verzenden van taken, nieuwe taak metagegevens en weergave oude metagegevens weergeven:

`Add-AdlaJobUser.ps1 -Account myadlsaccount -EntityToAdd 546e153e-0ecf-417b-ab7f-aa01ce4a7bff -EntityType User -FullReplication`


## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Aan de slag met Data Lake Analytics met behulp van de Azure-portal](data-lake-analytics-get-started-portal.md)
* [Azure Data Lake Analytics beheren met behulp van Azure PowerShell](data-lake-analytics-manage-use-powershell.md)


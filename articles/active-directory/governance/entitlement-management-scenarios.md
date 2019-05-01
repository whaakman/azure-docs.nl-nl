---
title: Algemene scenario's in Azure AD-recht beheer (Preview) - Azure Active Directory
description: Meer informatie over de belangrijke stappen die u voor veelvoorkomende scenario's in Azure Active Directory waar u recht op management (Preview volgen moet).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/23/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96442a9d49581da6841fa7acb8329354ec727f60
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/30/2019
ms.locfileid: "64918478"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management-preview"></a>Algemene scenario's in Azure AD waar u recht op beheer (Preview)

> [!IMPORTANT]
> Azure Active Directory (Azure AD) waar u recht op management is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Er zijn verschillende manieren waarop u recht management voor uw organisatie kunt configureren. Echter, als u net begint, is het handig om te begrijpen van de algemene scenario's voor beheerders, goedkeurders en aanvragers.

## <a name="administrators"></a>Beheerders

### <a name="im-new-to-entitlement-management-and-i-want-help-with-getting-started"></a>Ik ben bekend bent met het beheer van rechten en ik wil hulp bij het aan de slag

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | [Volg de zelfstudie voor het maken van uw eerste toegang-pakket](entitlement-management-access-package-first.md) | [![Pictogram van Azure portal](./media/entitlement-management-scenarios/azure-portal.png)](./media/entitlement-management-scenarios/azure-portal-expanded.png#lightbox) |

### <a name="i-want-to-allow-users-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Ik wil toestaan dat gebruikers in mijn directory voor het aanvragen van toegang tot groepen, toepassingen of SharePoint-sites

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Maak een nieuwe toegang-pakket in een catalogus](entitlement-management-access-package-create.md#start-new-access-package) | ![Een pakket toegang maken](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Resource-rollen voor toegang tot pakket toevoegen](entitlement-management-access-package-edit.md#add-resource-roles)<ul><li>Groepen</li><li>Applicaties</li><li>SharePoint-sites</li></ul> | ![Resource-rollen toevoegen](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Een beleid toevoegen](entitlement-management-access-package-edit.md#policy-for-users-in-your-directory)<ul><li>Voor gebruikers in uw directory</li><li>Goedkeuring vereisen</li><li>Instellingen voor het verloop</li></ul> | ![Beleid toevoegen](./media/entitlement-management-scenarios/policy.png) |

### <a name="i-want-to-allow-users-from-my-business-partners-including-users-not-yet-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Ik wilt toestaan dat gebruikers van mijn zakelijke partners (inclusief gebruikers nog niet in mijn map) toegang vragen tot groepen, toepassingen of SharePoint-sites

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Maak een nieuwe toegang-pakket in een catalogus](entitlement-management-access-package-create.md#start-new-access-package) | ![Een pakket toegang maken](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Resource-rollen voor toegang tot pakket toevoegen](entitlement-management-access-package-edit.md#add-resource-roles) | ![Resource-rollen toevoegen](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Een beleid voor externe gebruikers toevoegen](entitlement-management-access-package-edit.md#policy-for-users-not-in-your-directory)<ul><li>Voor gebruikers die niet in uw directory</li><li>Goedkeuring vereisen</li><li>Instellingen voor het verloop</li></ul> | ![Beleid voor externe gebruikers toevoegen](./media/entitlement-management-scenarios/policy-external.png) |
> | **4.** [De portal My Access-koppeling om aan te vragen van het pakket toegang tot uw zakelijke partner verzenden](entitlement-management-access-package-edit.md#copy-my-access-portal-link)<ul><li>Zakelijke partner kunt koppeling delen met gebruikers</li></ul> |  |

### <a name="i-want-to-change-the-groups-applications-or-sharepoint-sites-in-an-access-package"></a>Ik wil de groepen, toepassingen of SharePoint-sites in een pakket toegang wijzigen

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** Het pakket toegang openen | ![Resource-rollen toevoegen](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Toevoegen of verwijderen van de resource-rollen](entitlement-management-access-package-edit.md#add-resource-roles) | ![Resource-rollen toevoegen](./media/entitlement-management-scenarios/resource-roles-add.png) |

### <a name="i-want-to-view-who-has-an-assignment-to-groups-applications-or-sharepoint-sites"></a>Ik wil bekijken wie heeft een toewijzing aan groepen, toepassingen of SharePoint-sites

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** Open een access-pakket | ![Resource-rollen toevoegen](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Toewijzingen weergeven](entitlement-management-access-package-edit.md#view-who-has-an-assignment)<ul><li>Weergeven welke gebruikers toegang tot een access-pakket hebben</li><li>Weergave van die gebruiker toegang is verlopen</li></ul> |  |

### <a name="i-want-to-view-groups-applications-or-sharepoint-sites-a-user-has-access-to"></a>Ik wil groepen, toepassingen of een gebruiker toegang tot heeft SharePoint-sites weergeven

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | [Gebruikersrapport van de toewijzingen te bekijken](entitlement-management-reports.md)<ul><li>Weergave wanneer ze aangevraagd en wie heeft goedgekeurd</li></ul> |  |

## <a name="approvers"></a>Goedkeurders

### <a name="i-want-to-approve-requests-to-access-groups-applications-or-sharepoint-sites"></a>Ik wil goedkeuren van aanvragen voor toegang tot groepen, toepassingen of SharePoint-sites

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Openstaande aanvraag in mijn toegangsportal](entitlement-management-request-approve.md#open-request) | [![Mijn portal pictogram toegang](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** [Aanvraag voor toegang tot goedkeuren](entitlement-management-request-approve.md#approve-or-deny-request) | ![Toegang goedkeuren](./media/entitlement-management-scenarios/approve-access.png) |

## <a name="requestors"></a>Aanvragers

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-available-to-me-and-request-access"></a>Ik wil de groepen, toepassingen of SharePoint-sites aan mij weergeven en toegang aanvragen

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Aanmelden bij de portal My Access](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Mijn portal pictogram toegang](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Access-pakket vinden |  |
> | **3.** [Toegang aanvragen](entitlement-management-request-access.md#request-an-access-package) | ![Toegang aanvragen](./media/entitlement-management-scenarios/request-access.png) |

### <a name="im-an-external-user-and-i-want-to-request-access-to-groups-applications-or-sharepoint-sites-with-a-direct-link"></a>Ik ben een externe gebruiker en ik wil de toegang tot groepen, toepassingen of SharePoint-sites met een rechtstreekse koppeling

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [De portal koppeling die u hebt ontvangen van mijn toegang](entitlement-management-access-package-edit.md#copy-my-access-portal-link) |  |
> | **2.** [Aanmelden bij de portal My Access](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Mijn portal pictogram toegang](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **3.** [Toegang aanvragen](entitlement-management-request-access.md#request-an-access-package) | ![Externe gebruiker toegang aanvragen](./media/entitlement-management-scenarios/request-access-external.png) |

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-i-already-have-access-to"></a>Ik wil de groepen, toepassingen, of SharePoint-sites al toegang tot hebben weergeven

> [!div class="mx-tableFixed"]
> | Stappen | Voorbeeld |
> | --- | --- |
> | **1.** [Aanmelden bij de portal My Access](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Mijn portal pictogram toegang](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Weergave actieve access pakketten |  |

## <a name="next-steps"></a>Volgende stappen

- [Zelfstudie: Uw eerste toegang-pakket maken](entitlement-management-access-package-first.md)
- [Bewerken en beheren van een bestaand pakket met toegang](entitlement-management-access-package-edit.md)

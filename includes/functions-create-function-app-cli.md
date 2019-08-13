---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/05/2019
ms.author: glenga
ms.openlocfilehash: c90587ebd7b0a608e41f19ab600f82dc484bad0a
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949958"
---
## <a name="create-the-local-function-app-project"></a>Het lokale functie-app-project maken

Voer de volgende opdracht uit vanaf de opdrachtregel om een functie-app-project te maken in de map `MyFunctionProj` van de huidige lokale map. Er wordt ook een GitHub-opslagplaats gemaakt in `MyFunctionProj`.

```command
func init MyFunctionProj
```

Wanneer u hierom wordt gevraagd, selecteert u een runtime voor de werkrol uit de volgende taalopties:

+ `dotnet`: hiermee maakt u een .NET-klassebibliotheekproject (.csproj).
+ `node`: maakt een node. js-project. Kies ofwel `javascript` of `typescript`. 
+ `python`: voor een python-project moet u in plaats daarvan [een door http geactiveerde functie maken in azure](../articles/azure-functions/functions-create-first-function-python.md).
+ `powershell`: voor een Power Shell-project kunt u in plaats daarvan [uw eerste Power shell-functie maken in azure](../articles/azure-functions/functions-create-first-function-powershell.md). 


Nadat het project is gemaakt, gebruikt u de volgende opdracht om naar de nieuwe `MyFunctionProj` projectmap te gaan.

```command
cd MyFunctionProj
```

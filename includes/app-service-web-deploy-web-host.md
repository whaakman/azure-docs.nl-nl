### <a name="app-service-plan"></a>App Service-plan
Het service-abonnement voor het hosten van de web-app maakt. U geeft u de naam van het plan via de **hostingPlanName** parameter. De locatie van het plan is gebruikt voor de resourcegroep dezelfde locatie. De prijscategorie laag- en werkrollen grootte zijn opgegeven in de **sku** en **workerSize** parameters

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },


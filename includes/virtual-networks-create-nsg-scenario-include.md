## <a name="scenario"></a>Scenario
Dit document om beter te laten zien hoe u kunt nsg's maken, gebruikt het onderstaande scenario.

![VNet-scenario](./media/virtual-networks-create-nsg-scenario-include/figure1.png)

In dit scenario maakt u een NSG voor elk subnet in de **TestVNet** virtueel netwerk, zoals hieronder beschreven: 

* **NSG-FrontEnd**. De front-end NSG wordt toegepast op de *FrontEnd* subnet, en bevatten twee regels:    
  * **RDP-regel**. Deze regel kan de RDP-verkeer naar de *FrontEnd* subnet.
  * **Web-regel**. Deze regel kunnen HTTP-verkeer naar de *FrontEnd* subnet.
* **NSG-back-end**. De back-end NSG wordt toegepast op de *back-end* subnet, en bevatten twee regels:    
  * **SQL-regel**. Met deze regel kunnen SQL verkeer alleen via de *FrontEnd* subnet.
  * **Web-regel**. Deze regel niet alle internet gebonden verkeer van de *back-end* subnet.

De combinatie van deze regels maken een DMZ-achtige scenario, waarin de back-end-subnet kan alleen inkomend verkeer ontvangen voor SQL van de front-end-subnet en heeft geen toegang tot het Internet, terwijl de front-end-subnet kan met het Internet communiceren en ontvangen binnenkomende HTTP-aanvragen alleen.


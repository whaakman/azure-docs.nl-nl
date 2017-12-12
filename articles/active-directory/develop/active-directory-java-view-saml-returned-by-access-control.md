---
title: Weergave SAML geretourneerd door de Access Control-Service (Java)
description: Informatie over het weergeven van SAML geretourneerd door de Access Control-Service in Java-toepassingen die worden gehost op Azure.
services: active-directory
documentationcenter: java
author: rmcmurray
manager: mtillman
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: d239145806be19d2199314fa351d1121f52203c8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-view-saml-returned-by-the-azure-access-control-service"></a>SAML geretourneerd door de Azure Access Control-Service weergeven
Deze handleiding wordt beschreven hoe u om weer te geven van de onderliggende Security Assertion Markup Language (SAML) die aan uw toepassing wordt geretourneerd door de Azure Access Control Service (ACS). De handleiding is gebaseerd op de [Web gebruikers verifiëren met Azure Access Control-Service met behulp van Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) onderwerp, doordat de code die de SAML-informatie weergegeven. De voltooide toepassing ziet er ongeveer als volgt.

![Voorbeeld van SAML uitvoer][saml_output]

Zie voor meer informatie over ACS de [Vervolgstappen](#next_steps) sectie.

> [!NOTE]
> Het Azure Access Services besturingselement Filter is een community technology preview. Als de bètasoftware, wordt deze niet formeel ondersteund door Microsoft.
> 
> 

## <a name="prerequisites"></a>Vereisten
Voltooi de steekproef op voor het voltooien van de taken in deze handleiding [Web gebruikers verifiëren met Azure Access Control-Service met behulp van Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) en deze gebruiken als startpunt voor deze zelfstudie.

## <a name="add-the-jspwriter-library-to-your-build-path-and-deployment-assembly"></a>De bibliotheek JspWriter toevoegen aan uw build-pad en de implementatie-assembly
Toevoegen van de bibliotheek waarin de **javax.servlet.jsp.JspWriter** klasse naar uw build-pad en de implementatie-assembly. Als u Tomcat gebruikt, de bibliotheek is **jsp-api.jar**, die zich bevindt in de Apache **lib** map.

1. In de Projectverkenner van Eclipse met de rechtermuisknop op **MyACSHelloWorld**, klikt u op **pad**, klikt u op **configureren pad**, klikt u op de **bibliotheken**tabblad en klik vervolgens op **externe JARs toevoegen**.
2. In de **JAR selectie** dialoogvenster, gaat u naar het JAR nodig, selecteert u deze en klik op **Open**.
3. Met de **eigenschappen voor MyACSHelloWorld** dialoogvenster nog geopend, klikt u op **implementatie Assembly**.
4. In de **Web Deployment Assembly** dialoogvenster, klikt u op **toevoegen**.
5. In de **nieuwe Assembly richtlijn** dialoogvenster, klikt u op **Java Build Path vermeldingen** en klik vervolgens op **volgende**.
6. Selecteer de juiste bibliotheek en klik op **voltooien**.
7. Klik op **OK** sluiten de **eigenschappen voor MyACSHelloWorld** dialoogvenster.

## <a name="modify-the-jsp-file-to-display-saml"></a>Wijzigen van het JSP-bestand om SAML weer te geven
Wijzig **index.jsp** gebruiken de volgende code.

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print the text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out the child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate the names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish the sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process the child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate the child nodes of the doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-the-application"></a>De toepassing uitvoeren
1. Voer uw toepassing in de emulator van de computer of implementeren naar Azure met behulp van de stappen beschreven op [Web gebruikers verifiëren met Azure Access Control-Service met behulp van Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).
2. Een browser starten en open uw webtoepassing. Nadat u zich bij uw toepassing aanmelden, ziet u de SAML-informatie, waaronder de security assertion geleverd door de identiteitsprovider.

## <a name="next-steps"></a>Volgende stappen
Verder verkennen van ACS-functionaliteit en om te experimenteren met meer geavanceerde scenario's, Zie [Access Control Service 2.0][Access Control Service 2.0].

[Prerequisites]: #pre
[Modify the JSP file to display SAML]: #modify_jsp
[Add the JspWriter library to your build path and deployment assembly]: #add_library
[Run the application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How to Authenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png

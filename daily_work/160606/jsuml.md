## jsuml2 的例子

### jsuml2的结构

![](http://i.imgur.com/D93xbC5.png)
### jsuml2的例子
 1. showumltest.jsp 代码
 
		<%@ page language="java" contentType="text/html; charset=ISO-8859-1"
    	pageEncoding="ISO-8859-1"%>
    	<%@ taglib prefix="s" uri="/struts-tags"%>
		<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
		<html>
		<head>
    	<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <title>Example of a simple class diagram using the jsUML2 library</title>

      	<link type="text/css" rel="stylesheet" href="../jsuml2/Installation-Public/build/css/UDStyle.css" media="screen" />

    	<script type="text/javascript" src="../jsuml2/Installation-Public/build/UDCore.js"></script>
   		 <script type="text/javascript" src="../jsuml2/Installation-Public/build/UDModules.js"></script>


   		 <script type="text/javascript">
   			 window.onload = function(){

         	   var classDiagram = new UMLClassDiagram({id: 'classDiagram', width: 380, height: 300 });

            // Adding classes...
            var vehicleClass = new UMLClass({ x:100, y:50 });
            var carClass = new UMLClass({ x:30, y:170 });
            var boatClass = new UMLClass({ x:150, y:170 });
            classDiagram.addElement(vehicleClass);
            classDiagram.addElement(carClass);
            classDiagram.addElement(boatClass);
        
             // Adding generalizations...
            var generalization1 = new UMLGeneralization({ b:vehicleClass, a:carClass });
            var generalization2 = new UMLGeneralization({ b:vehicleClass, a:boatClass });
            classDiagram.addElement(generalization1);       
            classDiagram.addElement(generalization2);       


            //Defining vehicleClass
            vehicleClass.setName("Vehicle");
            vehicleClass.addAttribute( 'owner' );
            vehicleClass.addAttribute( 'capacity' );
            vehicleClass.addOperation( 'getOwner()' );
            vehicleClass.addOperation( 'getCapacity()' );
            
            //Defining carClass
            carClass.setName("Car");
            carClass.addAttribute( 'num_doors' );
            carClass.addOperation( 'getNumDoors()' );

            //Defining boatClass
            boatClass.setName("Boat");
            boatClass.addAttribute( 'mast' );
            boatClass.addOperation( 'getMast()' );

            //Draw the diagram
            classDiagram.draw();

            //Interaction is possible (editable)
            classDiagram.interaction(true);
        }
   		 </script>
    		</head>

   		 <body>
		<font size="2"><b>View <a href="class_code.html" target="_blank">code</a></b></font><br><br>

		<div id="classDiagram"></div>
   		 </body>
		</html>

2. 效果

![](http://i.imgur.com/p6RGtYW.png)
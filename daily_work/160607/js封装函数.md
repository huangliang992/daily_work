## 一个页面两次获多次调用相同js

## js封装还需再看看

代码封装成函数：将下面这段封装成函数,new 两个


	fuction diagram(id,cid,asid,agid,glid,dpid,cpid){
	var classDiagram = new UMLClassDiagram({
				id : id,
				width : 1340,
				height : 600
			});	
	var classes = document.getElementById(""+cid).value;
			if (classes != "") {
				alert(classes);
				var tt = eval("(" + classes + ")");
				//alert(tt);
				//用于保存class，用于连线
				var classtemp = {}
				//创建class
				for ( var k in tt) {
					var jt = tt[k];
					//已经获取到类JSON的对象了
					var cfi = new UMLClass({
						x : jt.x,
						y : jt.y
					});
					cfi.setName(jt.name);
					for (var i = 0; i < jt.attribute.length; i++) {
						cfi.addAttribute(jt.attribute[i]);
					}
					for (var i = 0; i < jt.operation.length; i++) {
						cfi.addOperation(jt.operation[i]);
					}
					classDiagram.addElement(cfi);
					var key = jt.id;
					//alert(key);
					classtemp[key] = cfi;
				}
			}
	
	//连Association关系
			var assr = document.getElementById(""+asid).value;
			if (assr != "") {
				var at = eval("(" + assr + ")");
				//alert(at);
				for ( var k in at) {
					var asj = at[k];
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == at[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == at[k].destid) {
							dest = classtemp[j];
						}
					}
					var association = new UMLAssociation({
						a : source,
						b : dest
					});
					classDiagram.addElement(association);
				}
			}

			//连泛化关系
			var glr = document.getElementById(""+glid).value;
			if (glr != "") {
				var gat = eval("(" + glr + ")");
				//alert(gat);
				for ( var k in gat) {
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == gat[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == gat[k].destid) {
							dest = classtemp[j];
						}
					}
					var generalization = new UMLGeneralization({
						a : source,
						b : dest
					});
					classDiagram.addElement(generalization);
				}
			}

			//连聚集关系
			var aggr = document.getElementById(""+agid).value;
			if (aggr != "") {
				var aat = eval("(" + aggr + ")");
				//alert(aat);
				for ( var k in aat) {
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == aat[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == aat[k].destid) {
							dest = classtemp[j];
						}
					}
					var aggregation = new UMLAggregation({
						a : dest,
						b : source
					});
					classDiagram.addElement(aggregation);
				}
			}

			//连依赖关系
			var dpr = document.getElementById(""+dpid).value;
			//alert(dpr);
			if (dpr != "") {//确保传入值不为空串
				//alert("yes");
				var dpat = eval("(" + dpr + ")");
				//alert(dpat);
				for ( var k in dpat) {
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == dpat[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == dpat[k].destid) {
							dest = classtemp[j];
						}
					}
					var dependency = new UMLDependency({
						a : source,
						b : dest
					});
					classDiagram.addElement(dependency);
				}
			}

			//连组合关系
			var cpr = document.getElementById(""+cpid).value;
			//alert(dpr);
			if (cpr != "") {//确保传入值不为空串
				//alert("yes");
				var cpat = eval("(" + cpr + ")");
				//alert(dpat);
				for ( var k in cpat) {
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == cpat[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == cpat[k].destid) {
							dest = classtemp[j];
						}
					}
					var composition = new UMLComposition({
						a : dest,
						b : source
					});
					classDiagram.addElement(composition);
				}
			}

			//Draw the diagram
			classDiagram.draw();

			//Interaction is possible (editable)
			classDiagram.interaction(true);


	}

原来的


	var classDiagram = new UMLClassDiagram({
				id : 'classDiagram1',
				width : 1340,
				height : 600
			});

			//var str=<s:property value="#session.filepath"/> ;
			//alert(str);
			//var imagepwd = document.getElementById("fp").value;
			//alert(imagepwd);
			
			//要加单引号
			var classes = document.getElementById("classes1").value;
			if (classes != "") {
				alert(classes);
				var tt = eval("(" + classes + ")");
				//alert(tt);
				//用于保存class，用于连线
				var classtemp = {}
				//创建class
				for ( var k in tt) {
					var jt = tt[k];
					//已经获取到类JSON的对象了
					var cfi = new UMLClass({
						x : jt.x,
						y : jt.y
					});
					cfi.setName(jt.name);
					for (var i = 0; i < jt.attribute.length; i++) {
						cfi.addAttribute(jt.attribute[i]);
					}
					for (var i = 0; i < jt.operation.length; i++) {
						cfi.addOperation(jt.operation[i]);
					}
					classDiagram.addElement(cfi);
					var key = jt.id;
					//alert(key);
					classtemp[key] = cfi;
				}
			}

			//连Association关系
			var assr = document.getElementById("ass1").value;
			if (assr != "") {
				var at = eval("(" + assr + ")");
				//alert(at);
				for ( var k in at) {
					var asj = at[k];
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == at[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == at[k].destid) {
							dest = classtemp[j];
						}
					}
					var association = new UMLAssociation({
						a : source,
						b : dest
					});
					classDiagram.addElement(association);
				}
			}

			//连泛化关系
			var glr = document.getElementById("gl1").value;
			if (glr != "") {
				var gat = eval("(" + glr + ")");
				//alert(gat);
				for ( var k in gat) {
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == gat[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == gat[k].destid) {
							dest = classtemp[j];
						}
					}
					var generalization = new UMLGeneralization({
						a : source,
						b : dest
					});
					classDiagram.addElement(generalization);
				}
			}

			//连聚集关系
			var aggr = document.getElementById("agg1").value;
			if (aggr != "") {
				var aat = eval("(" + aggr + ")");
				//alert(aat);
				for ( var k in aat) {
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == aat[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == aat[k].destid) {
							dest = classtemp[j];
						}
					}
					var aggregation = new UMLAggregation({
						a : dest,
						b : source
					});
					classDiagram.addElement(aggregation);
				}
			}

			//连依赖关系
			var dpr = document.getElementById("dp1").value;
			//alert(dpr);
			if (dpr != "") {//确保传入值不为空串
				//alert("yes");
				var dpat = eval("(" + dpr + ")");
				//alert(dpat);
				for ( var k in dpat) {
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == dpat[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == dpat[k].destid) {
							dest = classtemp[j];
						}
					}
					var dependency = new UMLDependency({
						a : source,
						b : dest
					});
					classDiagram.addElement(dependency);
				}
			}

			//连组合关系
			var cpr = document.getElementById("cp1").value;
			//alert(dpr);
			if (cpr != "") {//确保传入值不为空串
				//alert("yes");
				var cpat = eval("(" + cpr + ")");
				//alert(dpat);
				for ( var k in cpat) {
					var source;
					var dest;
					for ( var j in classtemp) {
						//alert(j);
						if (j == cpat[k].sourceid) {
							//alert(j);
							source = classtemp[j];
						}
						if (j == cpat[k].destid) {
							dest = classtemp[j];
						}
					}
					var composition = new UMLComposition({
						a : dest,
						b : source
					});
					classDiagram.addElement(composition);
				}
			}

			//Draw the diagram
			classDiagram1.draw();

			//Interaction is possible (editable)
			classDiagram1.interaction(true);

		}
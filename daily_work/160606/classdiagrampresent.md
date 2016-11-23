## 在将后台的数据转成JSON后传递给前台javascript 时遇到的问题
### 1.var classes='<s:property value="classes" escape="false"/>'

value="calsses" 中的classes是Action类中的JSON字符串，要加上escape，否则双引号会变成&quot，用eval（）解码字符串到JSON对象时解析不了

### 2. 在JS中测试变量用alert（）

### 3. JS中一样可以使用Struts标签，但要加单引号，否则也会出错

### 4. 用eval（）解析JSON字符串

### 4.遍历json中所有键值的value


### 类图展示的页面
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


    
    </head>

    <body>
	<script type="text/javascript">
		window.onload = function() {
			var classDiagram = new UMLClassDiagram({
				id : 'classDiagram',
				width : 1288,
				height : 600
			});

			//要加单引号
			var classes = '<s:property value="classes" escape="false"/>';
			if (classes != "") {
				//alert(classes);
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
			var assr = '<s:property value="ass" escape="false"/>';
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
			var glr = '<s:property value="gl" escape="false"/>';
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
			var aggr = '<s:property value="agg" escape="false"/>';
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
			var dpr = '<s:property value="dp" escape="false"/>';
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
			var cpr = '<s:property value="cp" escape="false"/>';
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
	</script>


	<div id="classDiagram"></div>
	<font size="2"><b>View <a href="default.jsp"
			target="_blank">code</a></b></font>
	<br>
	<h3 class="text-primary">The JSON of all the classes in the class diagram is</h3>
	<s:property value="classes" /><br><br>
	<h3 class="text-primary">The JSON of all the Association in the class diagram is</h3>
	<s:property value="ass" /><br>
	<h3 class="text-primary">The JSON of all the Aggregation in the class diagram is</h3>
	<s:property value="agg" /><br>
	<h3 class="text-primary">The JSON of all the Dependency in the class diagram is</h3>
	<s:property value="dp" /><br>
	<h3 class="text-primary">The JSON of all the Generalization in the class diagram is</h3>
	<s:property value="gl" /><br>
	<h3 class="text-primary">The JSON of all the Composition in the class diagram is</h3>
	<s:property value="cp" /><br>
	</body>
	</html>


### Action类

	package com.hainu.cs.action;

	import java.io.File;
	import java.io.FileInputStream;
	import java.io.FileOutputStream;
	import java.util.ArrayList;
	import java.util.HashMap;
	import java.util.Map;

	import org.apache.struts2.ServletActionContext;
	import org.json.JSONObject;

	import com.hainu.cs.circulationchecking.entity.AGFigure;
	import com.hainu.cs.circulationchecking.entity.ASFigure;
	import com.hainu.cs.circulationchecking.entity.CPFigure;
	import com.hainu.cs.circulationchecking.entity.ClassFigure;
	import com.hainu.cs.circulationchecking.entity.DPFigure;
	import com.hainu.cs.circulationchecking.entity.GLFigure;
	import com.hainu.cs.circulationchecking.permanant_storage.XMLManagerFul;
	import com.opensymphony.xwork2.ActionContext;
	import com.opensymphony.xwork2.ActionSupport;

	public class FileUploadAction extends ActionSupport{
	private File upload;
	private String uploadFileName;//XXXFileName is used to get the name of the file
	private String uploadContentType;// XXXContentType is used to get the type of the file
	private String statu="";
	
	
	private String classes;//类信息的JSON字符串
	private String ass;//关联的JSON字符串
	private String agg;//聚集关系的JSON字符串
	private String gl;//泛化关系的JSON字符串
	private String dp;//依赖关系的JSON字符串
	private String cp;
	
	
	public String getCp() {
		return cp;
	}


	public void setCp(String cp) {
		this.cp = cp;
	}


	public String getAss() {
		return ass;
	}


	public void setAss(String ass) {
		this.ass = ass;
	}


	public String getAgg() {
		return agg;
	}


	public void setAgg(String agg) {
		this.agg = agg;
	}


	public String getGl() {
		return gl;
	}


	public void setGl(String gl) {
		this.gl = gl;
	}


	public String getDp() {
		return dp;
	}


	public void setDp(String dp) {
		this.dp = dp;
	}


	public String getClasses() {
		return classes;
	}


	public void setClasses(String classes) {
		this.classes = classes;
	}

	public String getStatu() {
		return statu;
	}


	public void setStatu(String statu) {
		this.statu = statu;
	}


	public String execute() throws Exception{
		String path = ServletActionContext.getServletContext().getRealPath("/WEB-INF/upload");  
        String filename = path+File.separator+uploadFileName; 
        System.out.println(uploadFileName);
        System.out.println(filename);
        
        //session test
        ActionContext actionContext = ActionContext.getContext();   
        Map session = actionContext.getSession();   
        session.put("filepath", filename);   

        FileInputStream in = new FileInputStream(upload);  
        FileOutputStream out = new FileOutputStream(filename);  
        byte[]b = new byte[1024];  
        int len = 0;  
        while((len=in.read(b))>0){  
            out.write(b,0,len);  
        }  
        out.close();  
        
        XMLManagerFul xml=new XMLManagerFul();
        xml.init(filename);
        
        JSONObject cjb=new JSONObject();//类的JSON对象
        JSONObject asjb=new JSONObject();//关联
        JSONObject agjb=new JSONObject();
        JSONObject gljb=new JSONObject();
        JSONObject dpjb=new JSONObject();
        JSONObject cpjb=new JSONObject();
        
        HashMap<String,ClassFigure> cs=xml.getCls();
        for(Map.Entry<String, ClassFigure>entry:cs.entrySet()){
        	ClassFigure value=entry.getValue();
        	//System.out.println(value.getObjname());
        	JSONObject obj=new JSONObject();
        	obj.put("id", value.getId());
        	obj.put("name", value.getName());
        	obj.put("x", value.getX());
        	obj.put("y", value.getY());
        	obj.put("attribute", value.getAttr());
        	obj.put("operation", value.getOper());
        	//System.out.println(obj.toString());
        	cjb.put(value.getName(), obj);
        	classes=cjb.toString();
        }
        
        HashMap<String,ASFigure> as=xml.getAS();
        int k1=0;
        for(Map.Entry<String, ASFigure>entry:as.entrySet()){
        	k1++;
        	ASFigure value=entry.getValue();
        	JSONObject obj=new JSONObject();
        	obj.put("sourceid", value.getSourceId());
        	obj.put("destid", value.getDestId());
        	asjb.put("key"+k1,obj);
        	ass=asjb.toString();
        }
        
        
        HashMap<String,AGFigure> ag=xml.getAG();
        int k2=0;
        for(Map.Entry<String, AGFigure>entry:ag.entrySet()){
        	k2++;
        	AGFigure value=entry.getValue();
        	JSONObject obj=new JSONObject();
        	obj.put("sourceid", value.getSourceId());
        	obj.put("destid", value.getDestId());
        	agjb.put("key"+k2,obj);
        	agg=agjb.toString();
        }
        
        
        HashMap<String,GLFigure> gll=xml.getGL();
        int k3=0;
        for(Map.Entry<String, GLFigure>entry:gll.entrySet()){
        	k3++;
        	GLFigure value=entry.getValue();
        	JSONObject obj=new JSONObject();
        	obj.put("sourceid", value.getSourceId());
        	obj.put("destid", value.getDestId());
        	gljb.put("key"+k3,obj);
        	gl=gljb.toString();
        }
        
        HashMap<String,DPFigure> dep=xml.getDP();
        int k4=0;
        for(Map.Entry<String, DPFigure>entry:dep.entrySet()){
        	k4++;
        	DPFigure value=entry.getValue();
        	JSONObject obj=new JSONObject();
        	obj.put("sourceid", value.getSourceId());
        	obj.put("destid", value.getDestId());
        	dpjb.put("key"+k4,obj);
        	dp=dpjb.toString();
        }
        
        HashMap<String,CPFigure> cop=xml.getCP();
        int k5=0;
        for(Map.Entry<String, CPFigure>entry:cop.entrySet()){
        	k5++;
        	CPFigure value=entry.getValue();
        	JSONObject obj=new JSONObject();
        	obj.put("sourceid", value.getSourceId());
        	obj.put("destid", value.getDestId());
        	cpjb.put("key"+k5,obj);
        	cp=cpjb.toString();
        }
        
        System.out.println(classes.toString());
        System.out.println(ass);
        System.out.println(agg);
        System.out.println(gl);
        System.out.println(dp);
        System.out.println(cp);
        
        statu="SUCCESS";
        return "SUCCESS";  
	}


	public File getUpload() {
		return upload;
	}


	public void setUpload(File upload) {
		this.upload = upload;
	}


	public String getUploadFileName() {
		return uploadFileName;
	}


	public void setUploadFileName(String uploadFileName) {
		this.uploadFileName = uploadFileName;
	}


	public String getUploadContentType() {
		return uploadContentType;
	}


	public void setUploadContentType(String uploadContentType) {
		this.uploadContentType = uploadContentType;
	}

	
	
	}

### 效果
三个测试的例子

![](http://i.imgur.com/V0Th5CY.png)



![](http://i.imgur.com/fBmhZVA.png)


![](http://i.imgur.com/vVsIlGo.png)
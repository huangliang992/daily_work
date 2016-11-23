# 对重要的类的展示

## 1. XMLManagerFul 类解析uml类图，获取完整信息，区别于XMLManager只获取类名和id

	import java.util.ArrayList;
	import java.util.HashMap;
	import java.util.Map;

	import javax.xml.parsers.DocumentBuilder;
	import javax.xml.parsers.DocumentBuilderFactory;
	import javax.xml.parsers.ParserConfigurationException;
	import javax.xml.xpath.XPath;
	import javax.xml.xpath.XPathConstants;
	import javax.xml.xpath.XPathFactory;

	import org.hainu.cs.circulationchecking.entity.AGFigure;
	import org.hainu.cs.circulationchecking.entity.ASFigure;
	import org.hainu.cs.circulationchecking.entity.CPFigure;
	import org.hainu.cs.circulationchecking.entity.ClassFigure;
	import org.hainu.cs.circulationchecking.entity.DPFigure;
	import org.hainu.cs.circulationchecking.entity.GLFigure;
	import org.w3c.dom.Document;
	import org.w3c.dom.Element;
	import org.w3c.dom.Node;
	import org.w3c.dom.NodeList;
	/**
 	* 将uml中的完整信息提取出来，包括类的属性和方法，还有类的坐标，XMLManager没有将坐标和属性方法
 	* 等信息提取出来。这个类在后面操作类图抽象时要用到
 	* @author hl
 	*
 	*/
	public class XMLManagerFul {
	private HashMap<String,ClassFigure> cd=new HashMap<String,ClassFigure>();
	private HashMap<String,ASFigure> asd=new HashMap<String,ASFigure>();
	private HashMap<String,AGFigure> agd=new HashMap<String,AGFigure>();
	private HashMap<String,CPFigure> cpd=new HashMap<String,CPFigure>();
	private HashMap<String,GLFigure> gld=new HashMap<String,GLFigure>();
	private HashMap<String,DPFigure> dpd=new HashMap<String,DPFigure>();
	
	public void init(String filename) throws Exception{
			DocumentBuilderFactory factory=DocumentBuilderFactory.newInstance();
			DocumentBuilder builder=factory.newDocumentBuilder();
			Document doc=builder.parse(filename);
			Element root=doc.getDocumentElement();
			String expression="/uml/XMI/XMI.content/*[local-name()='Model']/*[local-name()='Namespace.ownedElement']";
			String expression1="/uml/pgml";
			XPathFactory xf=XPathFactory.newInstance();
			XPath xp=xf.newXPath();
				Element namespace=(Element) xp.evaluate(expression, doc, XPathConstants.NODE);
				Element pgml=(Element) xp.evaluate(expression1, doc, XPathConstants.NODE);
				System.out.println(namespace.getNodeName());
				for(Node node=namespace.getFirstChild();node!=null;node=node.getNextSibling()){
					if(node instanceof Element){
						if(node.getNodeName().equals("UML:Class")){
							String id=node.getAttributes().getNamedItem("xmi.id").getNodeValue();
							String name=node.getAttributes().getNamedItem("name").getNodeValue();
							ArrayList<String> at=new ArrayList<String>();
							ArrayList<String> op=new ArrayList<String>();
							for(Node node1=node.getFirstChild();node1!=null;node1=node1.getNextSibling()){
								if(node1 instanceof Element){
									if(node1.getNodeName().equals("UML:Classifier.feature")){
										NodeList attrlist=((Element) node1).getElementsByTagName("UML:Attribute");
										NodeList operlist=((Element) node1).getElementsByTagName("UML:Operation");
										
										for(int i=0;i<attrlist.getLength();i++){
											String a=attrlist.item(i).getAttributes().getNamedItem("name").getNodeValue();
											at.add(a);
										}
										for(int i=0;i<operlist.getLength();i++){
											String o=operlist.item(i).getAttributes().getNamedItem("name").getNodeValue();
											op.add(o);
										}
										
									}
									if(node1.getNodeName().equals("UML:Namespace.ownedElement")){
										NodeList list=((Element) node1).getElementsByTagName("UML:Dependency");
										Element node2=(Element) list.item(0);
										System.out.println(node2.getNodeName());
										if(node2!=null){
										String dpid=node2.getAttributes().getNamedItem("xmi.id").getNodeValue();
										NodeList list1=node2.getElementsByTagName("UML:Class");
										String sourceid=list1.item(0).getAttributes().getNamedItem("xmi.idref").getNodeValue();
										String destid=list1.item(1).getAttributes().getNamedItem("xmi.idref").getNodeValue();
										dpd.put(dpid, new DPFigure(sourceid,destid));
										}
									}
								}
							}
							cd.put(id, new ClassFigure(name,at,op));
						}
						if(node.getNodeName().equals("UML:Association")){
							String id=node.getAttributes().getNamedItem("xmi.id").getNodeValue();
							NodeList list=((Element) node).getElementsByTagName("UML:AssociationEnd");
							NodeList list1=((Element) node).getElementsByTagName("UML:Class");
							String sourceid=list1.item(0).getAttributes().getNamedItem("xmi.idref").getNodeValue();
							String destid=list1.item(1).getAttributes().getNamedItem("xmi.idref").getNodeValue();
							if(list.item(0).getAttributes().getNamedItem("aggregation").getNodeValue().equals("none")){
								asd.put(id, new ASFigure(sourceid,destid));
							}
							if(list.item(0).getAttributes().getNamedItem("aggregation").getNodeValue().equals("aggregate")){
								System.out.println("Aggregate");
								agd.put(id, new AGFigure(destid,sourceid));
							}
							if(list.item(0).getAttributes().getNamedItem("aggregation").getNodeValue().equals("composite")){
								cpd.put(id, new CPFigure(destid,sourceid));
							}
						}
						if(node.getNodeName().equals("UML:Generalization")){
							String id=node.getAttributes().getNamedItem("xmi.id").getNodeValue();
							NodeList list=((Element) node).getElementsByTagName("UML:Class");
							String sourceid=list.item(0).getAttributes().getNamedItem("xmi.idref").getNodeValue();
							String destid=list.item(1).getAttributes().getNamedItem("xmi.idref").getNodeValue();
							gld.put(id, new GLFigure(sourceid,destid));
						}
					}
				}
				for (Map.Entry<String, ClassFigure> entry : cd.entrySet()) {
					String key = entry.getKey();
					ClassFigure value = entry.getValue();
					for (Node node = pgml.getFirstChild(); node != null; node = node.getNextSibling()) {
						if (node instanceof Element) {
							if (node.getAttributes().getNamedItem("href").getNodeValue().equals(key)) {
								NodeList list=((Element) node).getElementsByTagName("rectangle");
								Node node1=list.item(0);
								String x=node1.getAttributes().getNamedItem("x").getNodeValue();
								String y=node1.getAttributes().getNamedItem("y").getNodeValue();
								int x1=Integer.parseInt(x);
								int y1=Integer.parseInt(y);
								value.setX(x1);;
								value.setY(y1);
							}
						}
					}
				}
			}		
	public HashMap<String,ASFigure> getAS(){
		return this.asd;
	}
	public HashMap<String,AGFigure> getAG(){
		return this.agd;
	}
	public HashMap<String,CPFigure> getCP(){
		return this.cpd;
	}
	public HashMap<String,GLFigure> getGL(){
		return this.gld;
	}
	public HashMap<String,DPFigure> getDP(){
		return this.dpd;
	}
	public HashMap<String,ClassFigure> getCls(){
		return this.cd;
	}
	}


## 2. PresentClassDiagram生成展示的图

	import java.util.HashMap;
	import java.util.Map;
	import org.eclipse.draw2d.Figure;
	import org.eclipse.draw2d.XYLayout;
	import org.eclipse.draw2d.geometry.Rectangle;
	import org.hainu.cs.circulationchecking.entity.AGFigure;
	import org.hainu.cs.circulationchecking.entity.ASFigure;
	import org.hainu.cs.circulationchecking.entity.CPFigure;
	import org.hainu.cs.circulationchecking.entity.ClassFigure;
	import org.hainu.cs.circulationchecking.entity.DPFigure;
	import org.hainu.cs.circulationchecking.entity.GLFigure;
	import org.hainu.cs.circulationchecking.permanant_storage.XMLManagerFul;
	/**
	 * 根据类和关系信息生成展示的图
 	* @author hl
 	*
 	*/
	public class PresentClassDiagram extends Figure{
	private HashMap<String,ASFigure> asf=new HashMap<String,ASFigure>();
	private HashMap<String,AGFigure> agf=new HashMap<String,AGFigure>();
	private HashMap<String,CPFigure> cpf=new HashMap<String,CPFigure>();
	private HashMap<String,DPFigure> dpf=new HashMap<String,DPFigure>();
	private HashMap<String,GLFigure> glf=new HashMap<String,GLFigure>();
	private HashMap<String,ClassFigure> clf=new HashMap<String,ClassFigure>();
	
	public void setAS(HashMap<String,ASFigure> asf){
		this.asf=asf;
	}
	
	public void setAG(HashMap<String,AGFigure> agf){
		this.agf=agf;
	}
	
	public void setCP(HashMap<String,CPFigure> cpf){
		this.cpf=cpf;
	}
	
	public void setDP(HashMap<String,DPFigure> dpf){
		this.dpf=dpf;
	}
	
	public void setGL(HashMap<String,GLFigure> glf){
		this.glf=glf;
	}
	
	public void setCls(HashMap<String,ClassFigure> clf){
		this.clf=clf;
	}
	
	public void init() throws Exception{
		
		generateFigure(asf,agf,cpf,dpf,glf,clf);
		
	}
	
	public void generateFigure(HashMap<String,ASFigure> asf, HashMap<String,AGFigure> agf,
			HashMap<String,CPFigure> cpf,HashMap<String,DPFigure> dpf,HashMap<String,GLFigure> glf,
			HashMap<String,ClassFigure>clf){
		
		//根据上述信息生成figure
		XYLayout layout=new XYLayout();
		this.setLayoutManager(layout);
		HashMap<String,ClassDiagram> array=new HashMap<String,ClassDiagram>();
		//int k=0;
		for(Map.Entry<String, ClassFigure>entry:clf.entrySet()){
			String key=entry.getKey();
			ClassFigure value=entry.getValue();
			ClassDiagram e=new ClassDiagram();
			array.put(key, e);
			//k++;
			if(value.getAttr().isEmpty()){
				e.addAttribute("        ");
			}
			if(value.getOper().isEmpty()){
				e.addOperation("        ");
			}
			for(int i=0;i<value.getAttr().size();i++){
				String a=value.getAttr().get(i);
				System.out.println(a);
				e.addAttribute(a);
			}
			for(int i=0;i<value.getOper().size();i++){
				String a=value.getOper().get(i);
				System.out.println(a);
				e.addOperation(a);
			}
			e.addName(value.getName());
			this.add(e);
			layout.setConstraint(e, new Rectangle(value.getX(),value.getY(),-1,-1));
		}	
		//make a judge whether really exist
		for(Map.Entry<String, ASFigure>entry:asf.entrySet()){
			ASFigure value=entry.getValue();
			ClassDiagram temp1=new ClassDiagram();
			ClassDiagram temp2=new ClassDiagram();
			int k=0;
			for(Map.Entry<String, ClassDiagram>entry1:array.entrySet()){
				String key=entry1.getKey();
				ClassDiagram value1=entry1.getValue();
				if(value.getSourceId().equals(key)){
					temp1=value1;
					k++;
				}
				if(value.getDestId().equals(key)){
					temp2=value1;
					k++;
				}
			}
			if(k==2){
			AssociationDiagram e=new AssociationDiagram(temp1,temp2);
			this.add(e);
			}
		}
		
		for(Map.Entry<String, AGFigure>entry:agf.entrySet()){
			AGFigure value=entry.getValue();
			ClassDiagram temp1=new ClassDiagram();
			ClassDiagram temp2=new ClassDiagram();
			System.out.println(value.getDestId()+"  "+value.getSourceId());
			int k=0;
			for(Map.Entry<String, ClassDiagram>entry1:array.entrySet()){
				String key=entry1.getKey();
				ClassDiagram value1=entry1.getValue();
				System.out.println(key);
				if(value.getSourceId().equals(key)){
					temp1=value1;
					k++;
				}
				if(value.getDestId().equals(key)){
					temp2=value1;
					k++;
				}
			}
			System.out.println(k);
			if(k==2){
			AggregationDiagram e=new AggregationDiagram(temp2,temp1);
			this.add(e);
			}
		}
		
		
		for(Map.Entry<String, CPFigure>entry:cpf.entrySet()){
			CPFigure value=entry.getValue();
			ClassDiagram temp1=new ClassDiagram();
			ClassDiagram temp2=new ClassDiagram();
			int k=0;
			for(Map.Entry<String, ClassDiagram>entry1:array.entrySet()){
				String key=entry1.getKey();
				ClassDiagram value1=entry1.getValue();
				if(value.getSourceId().equals(key)){
					temp1=value1;
					k++;
				}
				if(value.getDestId().equals(key)){
					temp2=value1;
					k++;
				}
			}
			if(k==2){
			CompositionDiagram e=new CompositionDiagram(temp2,temp1);
			this.add(e);
			}
		}
		
		
		for(Map.Entry<String, DPFigure>entry:dpf.entrySet()){
			DPFigure value=entry.getValue();
			ClassDiagram temp1=new ClassDiagram();
			ClassDiagram temp2=new ClassDiagram();
			int k=0;
			for(Map.Entry<String, ClassDiagram>entry1:array.entrySet()){
				String key=entry1.getKey();
				ClassDiagram value1=entry1.getValue();
				if(value.getSourceId().equals(key)){
					temp1=value1;
					k++;
				}
				if(value.getDestId().equals(key)){
					temp2=value1;
					k++;
				}
			}
			if(k==2){
			DependencyDiagram e=new DependencyDiagram(temp1,temp2);
			this.add(e);
			}
		}
		
		/////////////////////////////////////////////////////////////////////
		for(Map.Entry<String, GLFigure>entry:glf.entrySet()){
			GLFigure value=entry.getValue();
			ClassDiagram temp1=new ClassDiagram();
			ClassDiagram temp2=new ClassDiagram();
			int k=0;
			for(Map.Entry<String, ClassDiagram>entry1:array.entrySet()){
				String key=entry1.getKey();
				ClassDiagram value1=entry1.getValue();
				if(value.getSourceId().equals(key)){
					temp1=value1;
					k++;
				}
				if(value.getDestId().equals(key)){
					temp2=value1;
					k++;
				}
			}
			if(k==2){
			GeneralizationDiagram e=new GeneralizationDiagram(temp1,temp2);
			this.add(e);
			}
		}
		
	}
	}


## 3. ShowXMLFigure展示完整的（原始的类图）

	import java.util.HashMap;
	import java.util.Map;

	import org.eclipse.draw2d.Figure;
	import org.eclipse.draw2d.XYLayout;
	import org.eclipse.draw2d.geometry.Rectangle;
	import org.hainu.cs.circulationchecking.entity.AGFigure;
	import org.hainu.cs.circulationchecking.entity.ASFigure;
	import org.hainu.cs.circulationchecking.entity.CPFigure;
	import org.hainu.cs.circulationchecking.entity.ClassFigure;
	import org.hainu.cs.circulationchecking.entity.Classes;
	import org.hainu.cs.circulationchecking.entity.DPFigure;
	import org.hainu.cs.circulationchecking.entity.GLFigure;
	import org.hainu.cs.circulationchecking.entity.Relationships;
	import org.hainu.cs.circulationchecking.permanant_storage.XMLManager;
	import org.hainu.cs.circulationchecking.permanant_storage.XMLManagerFul;
	/**
 	* 根据XMLManagerFul提取出来的信息构造展示的类图Figure
 	* 这个类与XMLManagerFul类其实是将ShowFigure类拆分了
 	* @author hl
 	*
 	*/
	public class ShowXMLFigure extends PresentClassDiagram{
	
	public void initShow(String filepath) throws Exception{
		XMLManagerFul xmf=new XMLManagerFul();
		xmf.init(filepath);
		setAS(xmf.getAS());
		setAG(xmf.getAG());
		setCP(xmf.getCP());
		setDP(xmf.getDP());
		setGL(xmf.getGL());
		setCls(xmf.getCls());
		this.init();
	}
	
	}


## 4. ShowAbstractedClass展示rank值排在前1/3的类

	import java.util.HashMap;
	import java.util.Map;

	import org.hainu.cs.circulationchecking.controler.SortClass;
	import org.hainu.cs.circulationchecking.entity.AGFigure;
	import org.hainu.cs.circulationchecking.entity.ASFigure;
	import org.hainu.cs.circulationchecking.entity.CPFigure;
	import org.hainu.cs.circulationchecking.entity.ClassFigure;
	import org.hainu.cs.circulationchecking.entity.DPFigure;
	import org.hainu.cs.circulationchecking.entity.GLFigure;
	import org.hainu.cs.circulationchecking.permanant_storage.XMLManagerFul;

	public class ShowAbstractedClass extends PresentClassDiagram{
	private SortClass sc;
	private XMLManagerFul xf;
	private HashMap<String,ASFigure> asf=new HashMap<String,ASFigure>();
	private HashMap<String,AGFigure> agf=new HashMap<String,AGFigure>();
	private HashMap<String,CPFigure> cpf=new HashMap<String,CPFigure>();
	private HashMap<String,DPFigure> dpf=new HashMap<String,DPFigure>();
	private HashMap<String,GLFigure> glf=new HashMap<String,GLFigure>();
	private HashMap<String,ClassFigure> clf=new HashMap<String,ClassFigure>();
	
	public void genClInfo(String filepath) throws Exception{
		sc=new SortClass();
		sc.init(filepath);
		String node[]=sc.getNodeSorted();
		xf=new XMLManagerFul();
		xf.init(filepath);
		HashMap<String,ASFigure> asf1=xf.getAS();
		HashMap<String,AGFigure> agf1=xf.getAG();
		HashMap<String,CPFigure> cpf1=xf.getCP();
		HashMap<String,DPFigure> dpf1=xf.getDP();
		HashMap<String,GLFigure> glf1=xf.getGL();
		HashMap<String,ClassFigure> clf1=xf.getCls();
		int ab1=node.length/3;
		//将重要的类加入要展示的类HashMap集合中
		for(int i=0;i<ab1;i++){
			for(Map.Entry<String, ClassFigure>entry:clf1.entrySet()){
				String key=entry.getKey();
				ClassFigure value=entry.getValue();
				if(node[i].equals(value.getName())){
					System.out.println(node[i]);
					clf.put(key, value);
				}
			}
		}
	}
	
	public void genReInfo(){}
	
	//设置完类和关系信息后初始化，建立图形
	public void makeFigure() throws Exception{
		this.setCls(clf);
		this.setAG(agf);
		this.setAS(asf);
		this.setGL(glf);
		this.setCP(cpf);;
		this.setDP(dpf);
		this.init();
	}
	}

## 5. 展示的结果

### 5.1 原始的图

![](http://i.imgur.com/apNiZ3v.png)

### 5.2 提取出来的重要的类的展示

![](http://i.imgur.com/FrRxw54.png)

## 6. 后续要做的工作，找到提取出来的重要类之间的关系，通过一定的算法把他们连起来
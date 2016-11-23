# 依据相关度矩阵，优化后的关系矩阵，排序后的rank向量，展示抽象后的图形

## 1.例如
 
![](http://i.imgur.com/XXt0J3R.png)

结果

![](http://i.imgur.com/5CKHSUH.png)


## 2. 代码

	import java.util.HashMap;
	import java.util.Map;
	import java.util.Random;
	import org.hainu.cs.circulationchecking.controler.CountCorrelation;
	import org.hainu.cs.circulationchecking.controler.CreateGraph;
	import org.hainu.cs.circulationchecking.controler.SortClass;
	import org.hainu.cs.circulationchecking.entity.AGFigure;
	import org.hainu.cs.circulationchecking.entity.ASFigure;
	import org.hainu.cs.circulationchecking.entity.CPFigure;
	import org.hainu.cs.circulationchecking.entity.ClassFigure;
	import org.hainu.cs.circulationchecking.entity.DPFigure;
	import org.hainu.cs.circulationchecking.entity.GLFigure;
	import org.hainu.cs.circulationchecking.permanant_storage.XMLManagerFul;
	public class ShowAbstractedClass extends PresentClassDiagram{
	private SortClass sc=new SortClass();
	private CountCorrelation cc=new CountCorrelation();
	private CreateGraph cg=new CreateGraph();
	private XMLManagerFul xf=new XMLManagerFul();
	private HashMap<String,ASFigure> asf=new HashMap<String,ASFigure>();
	private HashMap<String,AGFigure> agf=new HashMap<String,AGFigure>();
	private HashMap<String,CPFigure> cpf=new HashMap<String,CPFigure>();
	private HashMap<String,DPFigure> dpf=new HashMap<String,DPFigure>();
	private HashMap<String,GLFigure> glf=new HashMap<String,GLFigure>();
	private HashMap<String,ClassFigure> clf=new HashMap<String,ClassFigure>();
	
	
	public void genInfo(String filepath) throws Exception{
		sc.init(filepath);
		String node[]=sc.getNodeSorted();//获取按rank从大到小的排序
		xf.init(filepath);
		cg.initGraph(filepath);
		String[] ghnode=cg.getNode();//生成的图的节点
		String [][] ghedge=cg.getEdge();//生成的图的边
		cc.init(ghnode, ghedge);
		float[][] correlation=cc.getCorrelation();//获取类之间的相关度矩阵
		String[][] type=cc.getRelationType();//获取类之间的关系矩阵
		HashMap<String,ASFigure> asf1=xf.getAS();
		HashMap<String,AGFigure> agf1=xf.getAG();
		HashMap<String,CPFigure> cpf1=xf.getCP();
		HashMap<String,DPFigure> dpf1=xf.getDP();
		HashMap<String,GLFigure> glf1=xf.getGL();
		HashMap<String,ClassFigure> clf1=xf.getCls();
		int ab1=node.length/3;//认为rank在前1/3的类是重要的
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
		//遍历相关度矩阵，如果相关度大于0.4则认为这两个类之间高度相关，要显示关系
		for(int i=0;i<ab1;i++){
			for(int j=0;j<ab1;j++){
				float corr=getCorOfTwo(node[i],node[j],correlation,ghnode);
				String relt=getRelOfTwo(node[i],node[j],type,ghnode);
				if(corr>0.4&relt!="None"){
					genRelation(node[i],node[j],relt);
				}
			}
		}
	}
	
	/**
	 * 查找任意两个重要的类之间的相关度
	 * @param row  要查row和colum之间相关度
	 * @param colum
	 * @param correlation 相关度表
	 * @param node 节点
	 * @return
	 */
	public float getCorOfTwo(String row,String colum,float[][] correlation,String[] node){
		int k1=-1;
		int k2=-1;
		for(int i=0;i<node.length;i++){
			if(row.equals(node[i])){
				k1=i;
				}
			if(colum.equals(node[i])){
				k2=i;
			}
			}	
		return correlation[k1][k2];
		}
	/**
	 * 查找任意两个重要的类之间的关系
	 * @param row
	 * @param colum
	 * @param reltp
	 * @param node
	 * @return
	 */
	public String getRelOfTwo(String row,String colum,String[][] reltp,String[] node){
		int k1=-1;
		int k2=-1;
		for(int i=0;i<node.length;i++){
			if(row.equals(node[i])){
				k1=i;
				}
			if(colum.equals(node[i])){
				k2=i;
			}
			}	
		return reltp[k1][k2];
	}
	
	/**
	 * 根据优化后的重要类之间的关系生成，相应的关系的对象
	 * @param row
	 * @param colum
	 * @param rel
	 */
	public void genRelation(String row,String colum,String rel){
		//依据类名查找类的id
		String rid="";
		String cid="";
		for(Map.Entry<String, ClassFigure>entry:clf.entrySet()){
			String key=entry.getKey();
			ClassFigure value=entry.getValue();
			String nm=value.getName();
			if(nm.equals(row)){
				rid=key;
			}
			if(nm.equals(colum)){
				cid=key;
			}
		}
		if(rel.equals("AS")){
			ASFigure afg=new ASFigure(rid,cid);
			asf.put(getRandomString(10), afg);
		}
		if(rel.equals("AG")){
			AGFigure agg=new AGFigure(rid,cid);
			agf.put(getRandomString(10), agg);
		}
		if(rel.equals("GL")){
			GLFigure glg=new GLFigure(rid,cid);
			glf.put(getRandomString(10), glg);
		}
		if(rel.equals("DP")){
			DPFigure dpg=new DPFigure(rid,cid);
			dpf.put(getRandomString(10), dpg);
		}
		if(rel.equals("AGr")){
			AGFigure agg=new AGFigure(cid,rid);
			agf.put(getRandomString(10), agg);
		}
		if(rel.equals("DPr")){
			DPFigure dpg=new DPFigure(cid,rid);
			dpf.put(getRandomString(10), dpg);
		}
		if(rel.equals("GLr")){
			GLFigure glg=new GLFigure(cid,rid);
			glf.put(getRandomString(10), glg);
		}
	}
	
	
	
	//随机生成序列码
	public static String getRandomString(int length) { //length表示生成字符串的长度
	    String base = "abcdefghijklmnopqrstuvwxyz0123456789";   
	    Random random = new Random();   
	    StringBuffer sb = new StringBuffer();   
	    for (int i = 0; i < length; i++) {   
	        int number = random.nextInt(base.length());   
	        sb.append(base.charAt(number));   
	    }   
	    return sb.toString();   
	 }   


	
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

## 3. 后面可以改进之处

指定查看前几个重要的类  
指定查看前x/y的重要的类  

	/**
	 * 查看前n个重要的类
	 * @param filepath
	 * @param n
	 * @throws Exception
	 */
	public void genInfo(String filepath, int n) throws Exception{
		sc.init(filepath);
		String node[]=sc.getNodeSorted();//获取按rank从大到小的排序
		xf.init(filepath);
		cg.initGraph(filepath);
		String[] ghnode=cg.getNode();//生成的图的节点
		String [][] ghedge=cg.getEdge();//生成的图的边
		cc.init(ghnode, ghedge);
		float[][] correlation=cc.getCorrelation();//获取类之间的相关度矩阵
		String[][] type=cc.getRelationType();//获取类之间的关系矩阵
		HashMap<String,ClassFigure> clf1=xf.getCls();
		int ab1=n;//查看前n个重要的类
		if(n>node.length){
			ab1=node.length;
		}
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
		//遍历相关度矩阵，如果相关度大于0.4则认为这两个类之间高度相关，要显示关系
		for(int i=0;i<ab1;i++){
			for(int j=0;j<ab1;j++){
				float corr=getCorOfTwo(node[i],node[j],correlation,ghnode);
				String relt=getRelOfTwo(node[i],node[j],type,ghnode);
				if(corr>0.4&relt!="None"){
					genRelation(node[i],node[j],relt);
				}
			}
		}
	}

例如指定了3个类就显示3个重要的类，和他们之间的关系

![](http://i.imgur.com/yTjbe2u.png)


界面修改

![](http://i.imgur.com/Rh2uUx8.png)
# 类之间相关度的计算
首先对于每种关系，定义类这个关系的一个重要性，如下所示，类和类之间的相关度依据这个计算。


		String []type={"GL","DP","AG","AS","GLr","DPr","AGr"};
		float [] weight={1,(float) 0.8,(float) 0.7,(float) 0.6,1,(float) 0.8,(float) 0.7};

例如：对于下面这条路径，Customer和Item之间的相关度就是AS（0.6）* AGr（0.7）* AS（0.6）=0.6* 0.7 *0.6=0.25200003

	Path: Customer--AS--Order--AGr--OrderDetail--AS--Item 
## 1.算法如下

	import java.util.ArrayList;
	import org.hainu.cs.circulationchecking.entity.PathOfG;
	/**
	 * 根据节点和节点之间的关系类型，与节点和节点之间的距离
 		* 计算节点与节点之间的相关度。相关度越高，说明这两个节点越相关。
 		* @author hl
	 *
	 */
	public class CountCorrelation {
	
	private float[][] corre;
	private String [][] reltype;
	/**
	 * 要找到任意连个节点之间的所有路径，然后对每一条路径，依据关系计算在这条路径上
	 * 两端节点的相关度。如果有多条路径取最大的相关度值
	 * @param node
	 * @param edge
	 * @return
	 */
	
	public float[][] countCorrelation(String[] node, String [][] edge){
		float[][] correlation = new float[node.length][node.length];
		String []type={"GL","DP","AG","AS","GLr","DPr","AGr"};
		float [] weight={1,(float) 0.8,(float) 0.7,(float) 0.6,1,(float) 0.8,(float) 0.7};
		for(int i=0;i<node.length;i++){
			for(int j=0;j<node.length;j++){
				float cor=0;
				FindPathsOfTwoNode f=new FindPathsOfTwoNode();
				f.findPaths(node, edge, node[i], node[j]);
				//找到两个节点之间的所有路径
				ArrayList<PathOfG> p=f.getPaths();
				//对每一条路径上两段节点的相关度计算
				for(int k=0;k<p.size();k++){
					PathOfG pg=p.get(k);
					float temp=1;
					for(int m=0;m<pg.getEdge().size();m++){
						for(int n=0;n<type.length;n++){
							if(pg.getEdge().get(m).equals(type[n])){
								//System.out.println("yes");
								temp=temp*weight[n];
								System.out.print(temp+" ");
							}
						}
						
						//获取最大的相关度	
					}
					if(temp>cor){
							cor=temp;
					}
					System.out.println(cor);
				}
				correlation[i][j]=cor;
			}
		}
		return correlation;
	}
	
	public static void main(String[] args){
		String filepath="C:/Users/hl/Desktop/example.uml";
		CreateGraph c=new CreateGraph();
		c.initGraph(filepath);
		CountCorrelation co=new CountCorrelation();
		float[][] f=co.countCorrelation(c.getNode(), c.getEdge());
		for(int i=0;i<f.length;i++){
			System.out.print("  "+c.getNode()[i]+" ");
		}
		System.out.println();
		for(int i=0;i<f.length;i++){
			System.out.print(c.getNode()[i]+" ");
			for(int j=0;j<f.length;j++){
				System.out.print(f[i][j]+" ");
			}
			System.out.println();
		}
	}
	}

对于下面例子
![](http://i.imgur.com/NDZ8icw.png)

相关度的计算结果如下所示

  		Customer   Order   Credit   Item   Payment   Check   OrderDetail   Cash 
	Customer 0.0 0.6 0.36 0.25200003 0.36 0.36 0.42000002 0.36 
	Order 0.6 0.0 0.6 0.42000002 0.6 0.6 0.7 0.6 
	Credit 0.36 0.6 0.0 0.25200003 1.0 1.0 0.42000002 1.0 
	Item 0.25200003 0.42000002 0.25200003 0.0 0.25200003 0.25200003 0.6 0.25200003 
	Payment 0.36 0.6 1.0 0.25200003 0.0 1.0 0.42000002 1.0 
	Check 0.36 0.6 1.0 0.25200003 1.0 0.0 0.42000002 1.0 
	OrderDetail 0.42000002 0.7 0.42000002 0.6 0.42000002 0.42000002 0.0 0.42000002 
	Cash 0.36 0.6 1.0 0.25200003 1.0 1.0 0.42000002 0.0 

相关度高说明这两个类之间的距离短，并且这两个类之间的关系是比较重要的关系（如泛化而非关联），但这并不能说明这连个类之间一定相关。
举个例子

![](http://i.imgur.com/HjCz0B9.png)

A和C之间的相关度是1 * 1=1，但A，c之间没有关系。最终的关系还得依据路径上的关系抽象的结果来判断。相关度只是用来决定怎么连线的问题。线的类型（是泛化，关联，依赖或者没有）得依据路径的关系抽象结果来定。
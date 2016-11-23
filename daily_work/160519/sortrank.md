# 对classrank进行排序
## 1. 插入排序
	public class SortClass {
	private ClassRank c;
	private String[] node;
	private float[] rank;
	
	public void init(String filepath){
		c=new ClassRank();
		c.init(filepath);
		String [] cla=c.getNode();
		float [] rk=c.getClassRank();
		sortRank(rk,cla);
		node=cla;
		rank=rk;
	}
	
	public float[] getRankSorted(){
		return this.rank;
	}
	public String[] getNodeSorted(){
		return this.node;
	}
	
	/**
	 * 
	 * @param rank
	 * 对rank进行插入排序
	 * @return
	 */
	public void sortRank(float [] rank,String [] node){
		//每次选一个最大的放在开头
		for(int i=0;i<rank.length;i++){
			for(int j=i+1;j<rank.length;j++){
				if(rank[j]>rank[i]){
					float temp=rank[i];
					rank[i]=rank[j];
					rank[j]=temp;
					String temp1=node[i];
					node[i]=node[j];
					node[j]=temp1;
				}
			}
		}
	}
	public static void main(String[] args){
		String filepath="C:/Users/hl/Desktop/test1.uml";
		SortClass s=new SortClass();
		s.init(filepath);
		for(int i=0;i<s.getRankSorted().length;i++){
			System.out.println(s.getNodeSorted()[i]+": "+s.getRankSorted()[i]+" " );
		}
	}
	}

# 2. 排序结果
排序前，迭代后的rank

	Class0 D  227.99265   Class1 E  206.70248   Class2 A  127.734795   Class3 G  77.65485   Class4 F  102.001816   Class5 C  63.5117   Class6 B  116.89591   Class7 H  77.50593   

排序后

	D: 227.99265 
	E: 206.70248 
	A: 127.734795 
	B: 116.89591 
	F: 102.001816 
	G: 77.65485 
	H: 77.50593 
	C: 63.5117 
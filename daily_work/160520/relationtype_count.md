# 类之间关系的矩阵

## 1. 通过路径抽象获取类图中两个类之间的关系，构造关系的矩阵

例如 构造后如下所示

![](http://i.imgur.com/NDZ8icw.png)

  		Customer   Order   Credit   Item   Payment   Check   OrderDetail   Cash 
	Customer None AS AS AS AS AS AS AS 
	Order AS None AS AS AS AS AGr AS 
	Credit AS AS None AS GL None AS None 
	Item AS AS AS None AS AS AS AS 
	Payment AS AS GLr AS None GLr AS GLr 
	Check AS AS None AS GL None AS None 
	OrderDetail AS AG AS AS AS AS None AS 
	Cash AS AS None AS GL None AS None 

## 2. 算法

	public String[][] countRelationType(String[] node, String [][] edge){
		String []type={"GL","DP","AG","AS","GLr","DPr","AGr"};
		float [] weight={1,(float) 0.8,(float) 0.7,(float) 0.6,1,(float) 0.8,(float) 0.7};
		String[][] retype=new String[node.length][node.length];
		for(int i=0;i<node.length;i++){
			for(int j=0;j<node.length;j++){
				//先找i，j之间所有的路径
				FindPathsOfTwoNode fp=new FindPathsOfTwoNode();
				fp.findPaths(node, edge, node[i], node[j]);
				ArrayList<PathOfG> ps=fp.getPaths();
				System.out.println(ps.size());
				//i,j之间最终的关系判断（到底选哪一条路径的结果-依据可信度，可信度相同的话，选关系强度大的）
				String finalretype="None";
				float finReliability=0;
				for(int k=0;k<ps.size();k++){
					AbstractPaths abs=new AbstractPaths();
					abs.absPath(ps.get(k));
					float rer=abs.getReliability();
					String rel="None";
					//最终有抽象的结果
					if(abs.getEdgeResult().size()==1){
						rel=abs.getEdgeResult().get(0);
						//System.out.println("yes");
					}
					//选取可信度最大的结果
					if(rer>finReliability){
						finReliability=rer;
						finalretype=rel;
					}
					//如果可信度相同，选取关系强度最大的
					if(rer==finReliability){
						for(int m=0;m<type.length;m++){
							if(rel.equals(type[m])){
								for(int n=0;n<type.length;n++){
									if(finalretype.equals(type[n])){
										if(weight[m]>weight[n]){
											finalretype=rel;
										}
									}
								}
							}
						}
					}
				}
				retype[i][j]=finalretype;
			}
		}
		return retype;
	}
	
构造的邻接矩阵是优化后的邻接矩阵。邻接矩阵中的关系符合下面的规则。  
1. 可信度是最大的
2. 关系强度是最大的
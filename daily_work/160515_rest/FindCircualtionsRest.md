# FindCirculationsContainN的rest实现

	import java.util.ArrayList;

	import javax.ws.rs.GET;
	import javax.ws.rs.Path;
	import javax.ws.rs.PathParam;
	import javax.ws.rs.Produces;
	import javax.ws.rs.core.Response;

	import org.hainu.cs.circulationchecking.controler.FindCirculationsContainsN;
	import org.hainu.cs.circulationchecking.entity.PathOfG;
	import org.json.JSONObject;

	@Path("/findCir")
	public class RequestForFindCirculations {
	@Path("{node}")
	@GET
	@Produces("application/json")
	public Response findCirculations(@PathParam("node") String nd){
		
		System.out.println(nd);
		String filepath="E:/circulation-checking-rest/src/resources/upload/a.uml";
		FindCirculationsContainsN f=new FindCirculationsContainsN();
		f.setNode(nd);
		f.findCir(filepath);
		
		ArrayList<PathOfG> p=f.getCirculation();
		
		JSONObject paths =new JSONObject();
		System.out.println(p.size());
		for(int i=0;i<p.size();i++){
			PathOfG pg=p.get(i);
			ArrayList<String> node=pg.getNode();
			ArrayList<String> edge=pg.getEdge();
			JSONObject pathn=new JSONObject();
			JSONObject pathe=new JSONObject();
			JSONObject path=new JSONObject();
			for(int j=0;j<node.size();j++){
				if(j<node.size()-1){
					pathn.append("class"+j, node.get(j));
					pathe.append("edge"+j, edge.get(j));
				}else pathn.append("class"+j, node.get(j));
			}
			path.append("node", pathn);
			path.append("edge", pathe);
			System.out.println(path);
			paths.append("path"+i, path);
		}
		//返回的是Json格式，但不可行所有做成了String类型
		String result=""+paths;
		return Response.status(200).entity(result).build();
		}
	}

和之前的FindPathsOfTwoNode的实现是一样的。

请求的URI http://localhost:8080/circulation-checking-rest/test/findCir/D
findCir是查找环服务的名称，D是输入的节点的名称（表示查找包含这个名字的所有的环）

输出为已经转化为了json格式

	{"path0":[{"node":[{"class6":["D"],"class5":["C"],"class4":["B"],"class3":["A"],"class2":["F"],"class1":["E"],"class0":["D"]}],"edge":[{"edge5":["CPr"],"edge3":["AS"],"edge4":["AG"],"edge1":["DP"],"edge2":["AS"],"edge0":["GL"]}]}],"path1":[{"node":[{"class5":["D"],"class4":["B"],"class3":["A"],"class2":["F"],"class1":["E"],"class0":["D"]}],"edge":[{"edge3":["AS"],"edge4":["AS"],"edge1":["DP"],"edge2":["AS"],"edge0":["GL"]}]}],"path2":[{"node":[{"class3":["D"],"class2":["B"],"class1":["C"],"class0":["D"]}],"edge":[{"edge1":["AGr"],"edge2":["AS"],"edge0":["CPr"]}]}]}

![](http://i.imgur.com/06PaHwH.png)
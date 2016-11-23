# FindClassRank的实现
代码

	import javax.ws.rs.GET;
	import javax.ws.rs.Path;
	import javax.ws.rs.Produces;
	import javax.ws.rs.core.Response;
	import org.hainu.cs.circulationchecking.controler.ClassRank;
	import org.json.JSONObject;

	@Path("findranks")
	public class RequestForClassRank {
	
	@GET
	@Produces("application/json")
	public Response findRanks(){
		String filepath="E:/ckandabs_rest_git/circulation-checking-rest/src/resources/upload/a.uml";
		ClassRank ck=new ClassRank();
		ck.init(filepath);
		float [] e=ck.getClassRank();
		String [] n=ck.getNode();
		JSONObject j=new JSONObject();
		for(int i=0;i<e.length;i++){
			j.append(""+n[i], e[i]);
		}
		String result=""+j;
		return Response.status(200).entity(result).build();
		}
	}

请求连接 http://localhost:8080/circulation-checking-rest/test/findranks

结果

	{"A":[127.734795],"B":[116.89591],"C":[63.5117],"D":[227.99265],"E":[206.70248],"F":[102.001816],"G":[77.65485],"H":[77.50593]}

截图：

![](http://i.imgur.com/VBZB3sN.png)
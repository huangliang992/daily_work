# FindRelationshipsOfTwoNode 的Rest实现

代码部分

	import java.util.ArrayList;
	import javax.ws.rs.GET;
	import javax.ws.rs.Path;
	import javax.ws.rs.PathParam;
	import javax.ws.rs.Produces;
	import javax.ws.rs.core.Response;
	import org.hainu.cs.circulationchecking.controler.AbstractPaths;
	@Path("/findRelation")
	public class RequestForAbsPaths {
	@Path("{begin}/{end}")
	@GET
	@Produces("application/json")
	public Response findRelationships(@PathParam("begin") String begin, @PathParam("end") String end){
		
		String filepath="E:/ckandabs_rest_git/circulation-checking-rest/src/resources/upload/a.uml";
		AbstractPaths e=new AbstractPaths();
		e.init(filepath, begin, end);
		ArrayList<String> r=e.getAbstractResults();
		String str="";
		for(int i=0;i<r.size();i++){
			str=str+r.get(i);
		}
		return Response.status(200).entity(str).build();
		//return null;
		}
	}
请求连接 http://localhost:8080/circulation-checking-rest/test/findRelation/E/F
找E，F之间的关系

结果

	For path: E--GLr--D--CPr--C--AGr--B--AS--A--AS--F
	Apply AGr x B x AS equals AS 100 get: E--GLr--D--CPr--C--AS--A--AS--F
	Apply AS x A x AS equals AS 100 get: E--GLr--D--CPr--C--AS--F
	Final reliability : 1.0
	For path: E--GLr--D--AS--B--AS--A--AS--F
	Apply AS x B x AS equals AS 100 get: E--GLr--D--AS--A--AS--F
	Apply AS x A x AS equals AS 100 get: E--GLr--D--AS--F
	Apply GLr x D x AS equals AS 70 get: E--AS--F
	Final reliability : 0.7
	For path: E--DP--F
	Final reliability : 1.0

![](http://i.imgur.com/Flpe9Xm.png)
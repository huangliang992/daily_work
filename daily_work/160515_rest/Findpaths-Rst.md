# 新建Rest处理查找两个类之间所有路径的请求
## 1. rest返回值类型可以是什么类型的json，xml还能是其他的吗？比如说ArrayList类型。
@Produces标记定义的是返回值的类型。@Produces，标注返回的MIME媒体类型，（ 注解标注，这个注解可以包含一组字符串，默认值是*/*，它指定REST 服务的响应结果的MIME 类型，例如：application/xml、application/json、image/jpeg 等），你                     也可以同时返回多种类型，但具体生成结果时使用哪种格式取决于ContentType。CXF 默认返回的是JSON 字符串。

## 2. GBK和UTF-8乱码问题
首先导入的文件成为了乱码是因为workspace的编码方式不对 linux下默认编码是UTF-8，windows的是GBK，

那么怎么修改workspace的编码方式呢 很简单在window----preference---workspace可以设置只要把编码改成utf-8就可以喽

## 3. 问题： A message body writer for Java type, class net.sf.json.JSONObject, and MIME media type, application/json, was not found

返回类型return Response.status(200).entity(result).build();
result 是JSONObject类型的话不行。


## 4. FindPathsOfTwoNode的rest实现

	import java.util.ArrayList;

	import javax.ws.rs.GET;
	import javax.ws.rs.Path;
	import javax.ws.rs.PathParam;
	import javax.ws.rs.Produces;
	import javax.ws.rs.core.Response;
	import org.hainu.cs.circulationchecking.controler.FindPathsOfTwoNode;
	import org.hainu.cs.circulationchecking.entity.PathOfG;
	import org.json.JSONObject;
	@Path("/findpaths")
	public class RequestForFindPaths {
	@Path("{begin}/{end}")
	@GET
	@Produces("application/json")
	public Response findpaths(@PathParam("begin") String begin, @PathParam("end") String end){
		
		
		String filepath="E:/circulation-checking-rest/src/resources/upload/a.uml";
		FindPathsOfTwoNode f=new FindPathsOfTwoNode();
		
		f.setBegin(begin);
		System.out.println(begin+" "+end);
		f.setEnd(end);
		f.findPathsOfG(filepath);
		ArrayList<PathOfG> p=f.getPaths();
		
		JSONObject paths =new JSONObject();
		System.out.println("yes");
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
		System.out.println(paths);
		String result=""+paths;
		return Response.status(200).entity(result).build();
		
		//return Response.status(200).entity("yes").build();
		}
	}

请求的连接：http://localhost:8080/circulation-checking-rest/test/findpaths/Order/Credit

![](http://i.imgur.com/z5PAlm8.png)
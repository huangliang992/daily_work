##前端接受的数据的格式（复杂类型的）
（1）JSON格式，JSON的数组格式，如下先将一般的对象转化成JSON对象


		List<EducationRecord> elist=edi.queryEducationRecord();
				List<JSONObject> jlist=new ArrayList<JSONObject>();
				for(int i=0;i<elist.size();i++){
					EducationRecord er=elist.get(i);
					JSONObject json=new JSONObject();
					json.append("username", er.getUsername());
					json.append("time", er.getSdate().getTime());
					json.append("content", er.getContent());
					jlist.add(json);
				}
				mav.addObject("erecord", jlist);

JSON数据的样子，数组里面是JSON

		var education=
		[
		{"time":[1480218231000],"content":["内容1"],"username":["admin"]}, 
		{"time":[1480218246000],"content":["内容2"],"username":["admin"]}
		];

前端解析

		for(var i in education){
			content=content+"<p>"+education[i].username+"</p>";
			}

（2）document ready function在一个jsp页面中可以存在多个

（3）最后后台传递的数据在前台处理展示，而不要在后台生成带html标记的字符串后，直接传给前台，实现前后端分离
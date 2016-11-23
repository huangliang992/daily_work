## 类图显示

	package org.hainan.cs.controller;
	
	import java.io.IOException;
	import java.util.HashMap;
	import java.util.Map;
	
	import org.hainan.cs.classdiagram.AGFigure;
	import org.hainan.cs.classdiagram.ASFigure;
	import org.hainan.cs.classdiagram.CPFigure;
	import org.hainan.cs.classdiagram.ClassFigure;
	import org.hainan.cs.classdiagram.DPFigure;
	import org.hainan.cs.classdiagram.GLFigure;
	import org.hainan.cs.rules.GenerateDiagram;
	import org.json.JSONObject;
	import org.springframework.stereotype.Controller;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.RequestMethod;
	import org.springframework.web.servlet.ModelAndView;
	
	import opennlp.tools.util.InvalidFormatException;
	
	/**
	 * 接受和处理页面传过来的需求
	 * @author hl
	 *
	 */
	@Controller
	@RequestMapping(value="/demand")
	public class DToC {
		
		private String cls;
		private String ass;
		private String ags;
		private String gls;
		private String dps;
		private String cps;
		
		@RequestMapping
		public ModelAndView index(){
			ModelAndView mad=new ModelAndView("demandToModel");
			mad.addObject("classes", cls);
			mad.addObject("as", ass);
			mad.addObject("ag", ags);
			mad.addObject("gl", gls);
			mad.addObject("cp",cps);
			mad.addObject("dp",dps);
			return mad;
		}
		@RequestMapping(value="/addDemand",method = RequestMethod.POST)
		public ModelAndView buildDiagram(String demand) throws InvalidFormatException, IOException{
			System.out.println(demand);
			GenerateDiagram gd=new GenerateDiagram();
			gd.init(demand);
			HashMap<String,ClassFigure> clf=gd.getCf();
			HashMap<String,ASFigure> asf=gd.getAsf();
			HashMap<String,AGFigure> agf=gd.getAgf();
			HashMap<String,GLFigure> glf=gd.getGlf();
			HashMap<String,DPFigure> dpf=gd.getDpf();
			HashMap<String,CPFigure> cpf=gd.getCpf();
			JSONObject clj1=new JSONObject();
			JSONObject asj1=new JSONObject();
			JSONObject agj1=new JSONObject();
			JSONObject glj1=new JSONObject();
			JSONObject dpj1=new JSONObject();
			JSONObject cpj1=new JSONObject();
			for(Map.Entry<String, ClassFigure>entry:clf.entrySet()){
				String key=entry.getKey();
				ClassFigure value=entry.getValue();
				JSONObject clj=new JSONObject();
				clj.put("id", key);
				clj.put("name", value.getClassname());
				clj.put("attribute", value.getAttribute());
				clj.put("method", value.getOperation());
				clj.put("X", value.getX());
				clj.put("Y", value.getY());
				clj1.put(key, clj);
			}
			for(Map.Entry<String, ASFigure>entry:asf.entrySet()){
				String key=entry.getKey();
				ASFigure value=entry.getValue();
				JSONObject asj=new JSONObject();
				asj.put("id", key);
				asj.put("sourceId", value.getSourceId());
				asj.put("sourceName", value.getSourceName());
				asj.put("destId", value.getDestId());
				asj.put("destName", value.getDestName());
				asj1.put(key, asj);
				
			}
			for(Map.Entry<String, GLFigure>entry:glf.entrySet()){
				String key=entry.getKey();
				GLFigure value=entry.getValue();
				JSONObject glj=new JSONObject();
				glj.put("id", key);
				glj.put("sourceId", value.getSourceid());
				glj.put("sourceName", value.getSourcename());
				glj.put("destId", value.getDestid());
				glj.put("destName", value.getDestname());
				glj1.put(key, glj);
			}
			for(Map.Entry<String, AGFigure>entry:agf.entrySet()){
				String key=entry.getKey();
				AGFigure value=entry.getValue();
				JSONObject agj=new JSONObject();
				agj.put("id", key);
				agj.put("sourceId", value.getSourceid());
				agj.put("sourceName", value.getSourcename());
				agj.put("destId", value.getDestid());
				agj.put("destName", value.getDestname());
				agj1.put(key, agj);
			}
			for(Map.Entry<String, DPFigure>entry:dpf.entrySet()){
				String key=entry.getKey();
				DPFigure value=entry.getValue();
				JSONObject dpj=new JSONObject();
				dpj.put("id", key);
				dpj.put("sourceId", value.getSourceid());
				dpj.put("sourceName", value.getSourcename());
				dpj.put("destId", value.getDestid());
				dpj.put("destName", value.getDestname());
				dpj1.put(key, dpj);
			}
			for(Map.Entry<String, CPFigure>entry:cpf.entrySet()){
				String key=entry.getKey();
				CPFigure value=entry.getValue();
				JSONObject cpj=new JSONObject();
				cpj.put("id", key);
				cpj.put("sourceId", value.getSourceid());
				cpj.put("sourceName", value.getSourcename());
				cpj.put("destId", value.getDestid());
				cpj.put("destName", value.getDestname());
				cpj1.put(key, cpj);
			}
			cls=clj1.toString();
			ass=asj1.toString();
			ags=agj1.toString();
			gls=glj1.toString();
			dps=dpj1.toString();
			cps=cpj1.toString();
			ModelAndView mad=new ModelAndView("redirect:/demand");
			return mad;
		}
	}


![](http://i.imgur.com/dOdEWKh.jpg)


结果

![](http://i.imgur.com/Chrve4u.jpg)

检查发现构造应该没错。
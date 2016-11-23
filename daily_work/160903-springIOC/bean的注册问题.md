### Spring 基于java类的配置

### 配置类
    package org.hainan.cs.bean;
    
    import org.springframework.context.annotation.Bean;
    import org.springframework.context.annotation.Configuration;
    import org.springframework.context.annotation.Scope;
    
    @Configuration
    public class OpConf {
    	@Scope("singleton")
    	@Bean
    	public OpennlpPath opp(){
    		OpennlpPath op=new OpennlpPath();
    		op.setA("aaa");
    		return op;
    	}
    }

### bean类

    package org.hainan.cs.bean;
    
    public class OpennlpPath {
    	private String path;
    	private String a;
    
    	public String getA() {
    		return a;
    	}
    
    	public void setA(String a) {
    		this.a = a;
    	}
    
    	public String getPath() {
    		return path;
    	}
    
    	public void setPath(String path) {
    		this.path = path;
    	}
    	
    }
    
### 调用该类

    AnnotationConfigApplicationContext ctx=new AnnotationConfigApplicationContext(OpConf.class);
    		OpennlpPath opt=ctx.getBean(OpennlpPath.class);
    		opt.setPath(path);
    		opt.setA("bbb");
    		System.out.println("2:"+opt.getPath()+" "+opt.getA());

	
	ApplicationContext ctx=new AnnotationConfigApplicationContext(OpConf.class);
		OpennlpPath op=ctx.getBean(OpennlpPath.class);
		System.out.println("3:"+op.getPath()+" "+op.getA());

得出结果不一致，如何保存修改的结果重新注册bean？。
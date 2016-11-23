### 方形Layout
    package org.hainan.cs.rules;
    /**
     * 根据总的类的个数确定每个类的x，y
     * @author hl
     *
     */
    public class LayoutRule {
    	
    	private int x;
    	private int y;
    	public int getX() {
    		return x;
    	}
    	public void setX(int x) {
    		this.x = x;
    	}
    	public int getY() {
    		return y;
    	}
    	public void setY(int y) {
    		this.y = y;
    	}
    	//总共total个类，a表示这里面第a个类的坐标
    	//正方形布局
    	public void genXandY(int total, int a){
    		 int m=total/4;
    		 int n=total % 4;
    		 if(n!=0){
    			 m=m+1;
    			 }
    		 if(a%4==0){
    			 x=100+a/4*100;
    			 y=0;
    		 }else if(a%4==1){
    			 x=0;
    			 y=100+a/4*100;
    		 }else if(a%4==2){
    			 x=100+m*100;
    			 y=100+a/4*100;
    		 }else if(a%4==3){
    			 y=100+m*100;
    			 x=100+a/4*100;
    		 }
    	}
    	
    	public static void main(String[] args){
    		LayoutRule lt=new LayoutRule();
    		lt.genXandY(9, 7);
    		System.out.println("x="+lt.getX()+" y="+lt.getY());
    	}
    }
    

### 测试

![](http://i.imgur.com/SXOpTvS.jpg)

输出的类图排列

![](http://i.imgur.com/NH9igXr.jpg)

###还是不好看，看能不能弄成圆形布局
# 1、Mapper中递归场情

```xml
	<resultMap type="com.fwy.corebasic.entity.Core_Dept" id="deptChildrenMap">
		<result property="id" column="ID"/>
		<result property="parentId" column="PARENTID"/>
		<result property="parentName" column="PARENTNAME"/>
		<result property="orderCode" column="ORDERCODE"/>
		<result property="isSys" column="ISSYS"/>
		<result property="deptName" column="DEPTNAME"/>
		<result property="adminRegionId" column="ADMINREGIONID"/>
		<result property="isShow" column="ISSHOW"/>
		<result property="remark" column="REMARK"/>
		<result property="deptCode" column="DEPTCODE"/>
		<result property="createTime" column="DEPTPHONE"/>

		<collection column="id" property="children"  
					ofType="com.fwy.corebasic.entity.Core_Dept"
					select="getChildrenList"/>
	</resultMap>
	
	<select id="getChildrenList" resultMap="deptChildrenMap">
		select * from core_dept 
		where parentid = #{parentId} and isshow = 1
		order by ordercode
	</select>
	
	<select id="getTree" resultMap="deptChildrenMap">
		select * from core_dept 
		where isshow = 1 
			<if test="id !=null">
				and id = #{id}
			</if>
			<if test="id ==null">
				and parentid = 0
			</if>
		order by ordercode
	</select>
```

# 2、多级菜单递归实现多级分类 

```java
 @RequestMapping("appletConfigList")
    @Description("小程序配置list")
    public ModelAndView driverList(Driver driver, HttpServletRequest request) {
        //项目网络路径  nginx转发会丢失host 和 端口号  需要再配置文件进行修改
        //request.getSchema()可以返回当前页面使用的协议，http 或是 https
        //request.getServerName()可以返回当前页面所在的服务器的名字
        //request.getServerPort()可以返回当前页面所在的服务器使用的端口,就是80;
        //request.getContextPath()可以返回当前页面所在的应用的名字;
        System.out.println("返回当前页面使用的协议=request.getSchema()::" + request.getScheme());
        System.out.println("返回当前页面所在的服务器的名字=request.getServerName()::" + request.getServerName());
        System.out.println("返回当前页面所在的服务器使用的端口=request.getServerPort()::" + request.getServerPort());
        System.out.println("返回当前页面所在的应用的名字=request.getContextPath()::" + request.getContextPath());
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("workBench/appletConfig/appletConfigList");
        List<Core_Dept> list = deptService.list(null, null, null);
        list = sortByGrade(list);
        int i  = 100;

        return modelAndView;
    }

    //这个方法相当于返回一级菜单的List集合

    private List<Core_Dept> sortByGrade(List<Core_Dept> list) {
        List<Core_Dept> collect = list.stream()

                //过滤掉父结点不为0的结点
            	//Lambda若只有一个参数,()可以省略
            	//Lambda若中有一条语句时，return(若有)与{}若有都可以省略
                .filter(root -> root.getParentId() == 0)

//               <R> Stream<R> map(Function<? super T, ? extends R> mapper);
//                public interface Function<T, R> {   R apply(T t); }
                //R相当于Core_Dept


                //查找子子菜单并赋值到children的属性
                .map((root) -> {
                    root.setChildren(getChildren(root, list));
                    return root;
                }).collect(Collectors.toList());
        return collect;
    }

    //递归获取子菜单并给children属性赋值

    private List<Core_Dept> getChildren(Core_Dept root, List<Core_Dept> list) {
        List<Core_Dept> collect = list.stream()
                
                //找出以root为父结点的菜单
            	//
                .filter(e -> e.getParentId().longValue() == root.getId())
                
                //递归获取子菜单并给children属性赋值
                
                .map((e) -> {
                    e.setChildren(getChildren(e, list));
                    return e;
                }).collect(Collectors.toList());
        return collect;
    }

```


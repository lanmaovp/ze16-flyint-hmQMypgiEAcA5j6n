[合集 \- OverallAuth2\.0 权限管理系统(14\)](https://github.com)[1\.从0到1搭建权限管理系统系列四 .net8 中Autofac的使用（附源码）09\-29](https://github.com/cyzf/p/18439606)[2\.从0到1搭建权限管理系统系列一 .net8 使用Swagger（附当前源码）09\-12](https://github.com/cyzf/p/18410483)[3\.从0到1搭建权限管理系统系列二 .net8 使用JWT鉴权（附当前源码）09\-18](https://github.com/cyzf/p/18417965)[4\.从0到1搭建权限管理系统系列三 .net8 JWT创建Token并使用09\-23](https://github.com/cyzf/p/18422784)[5\.（系列五）.net8 中使用Dapper搭建底层仓储连接数据库（附源码）10\-06](https://github.com/cyzf/p/18448855)[6\.（系列六）.net8 全局异常捕获机制10\-12](https://github.com/cyzf/p/18460420)[7\.（系列七）.net8 Aop切面编程10\-16](https://github.com/cyzf/p/18467560)[8\.（系列八）.net8 webApi后端框架轮子，欢迎下载。10\-19](https://github.com/cyzf/p/18475954):[milou加速器](https://xinminxuehui.org)[9\.（系列九）使用Vue3\+Element Plus创建前端框架（附源码）10\-24](https://github.com/cyzf/p/18496583)[10\.（系列十）Vue3中菜单和路由的结合使用，实现菜单的动态切换（附源码）10\-30](https://github.com/cyzf/p/18502316)[11\.（系列十一）Vue3框架中路由守卫及请求拦截（实现前后端交互）11\-13](https://github.com/cyzf/p/18531946)[12\.（系列十二）Vue3\+.Net8实现用户登录（超详细登录文档）11\-25](https://github.com/cyzf/p/18560576)[13\.（系列十三）Vue3\+Echarts搭建超好看的系统面板12\-02](https://github.com/cyzf/p/18578342)14\.（系列十四）Vue3\+WebApi 搭建动态菜单12\-10收起
**说明**


    该文章是属于OverallAuth2\.0系列文章，每周更新一篇该系列文章（从0到1完成系统开发）。


    该系统文章，我会尽量说的非常详细，做到不管新手、老手都能看懂。


    说明：OverallAuth2\.0 是一个简单、易懂、功能强大的权限\+可视化流程管理系统。


友情提醒：本篇文章是属于系列文章，看该文章前，建议先看之前文章，可以更好理解项目结构。


**qq群：801913255，进群有什么不懂的尽管问，群主都会耐心解答。**


**有兴趣的朋友，请关注我吧(\*^▽^\*)。**


**![](https://img2024.cnblogs.com/blog/1158526/202408/1158526-20240824140446786-404771438.png)**


**关注我，学不会你来打我**


**问题修复**


说明：不是跟着系列文章搭建系统的请自行忽略，往下看搭建动态菜单的过程。


　**问题1：**


![](https://img2024.cnblogs.com/blog/1158526/202412/1158526-20241204131959863-1859974101.png)


修改代码如下


路径：framework\-\>index.vue


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
<el-main>
          <el-affix :offset="60">
            <el-tabs
              v-if="tabsList.length > 0"
              v-model="defaultActive"
              class="demo-tabs"
              @click="tabsClick(defaultActive)"
              @tab-remove="tabRemoveClick"
            >
              <el-tab-pane
                v-for="item in tabsList"
                :label="item.name"
                :name="item.path"
                :key="item.path"
                :closable="item.path == '/panel' ? false : true"
                style="font-size: 16px"
              >
              el-tab-pane>
            el-tabs>
          el-affix>
          <router-view>router-view>
        el-main>
```


View Code
样式.demo\-tabs中加入白色背景样式：background\-color: white;


　　**问题2：**


中国地图中，波纹会随着各省的数值变大而变大


![](https://img2024.cnblogs.com/blog/1158526/202412/1158526-20241204132755037-1894610160.png)


路径：echarts.ts


把value: chinaGeoCoordMap\[dataItem\[0].name].concat(\[dataItem\[0].value])修改成value: chinaGeoCoordMap\[dataItem\[0].name].concat(0\)


**实现功能**


把以下菜单换成动态菜单


　　不是跟着系列走的朋友，可直接观看动态路由菜单的关键代码 ，在后面的第三步中！！！


　　注意：该篇文章是实现OverallAuth2\.0 功能级权限之一，菜单权限的重要篇幅。


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
routes.push(
  {
    path: '/framework',
    component: Framework,
    name: "架构",

  },
  {
    path: '/login',
    component: Login,
    name: "登录页面",
  },
  {
    path: '/panel',
    redirect: '/panel/index',
    meta: { title: '工作空间' },
    name: "工作空间",
    component: Framework,
    children: [
      {
        path: '/panel',
        name: '工作台',
        component: () => import('../../views/panel/index.vue'),
        meta: { title: '工作台', requireAuth: true, affix: true, closable: false },
      }
    ]
  },
  {
    path: '/menu',
    redirect: '/menu/index',
    meta: { title: '菜单管理' },
    name: "菜单管理",
    component: Framework,
    children: [
      {
        path: '/menu',
        name: '菜单',
        component: () => import('../../views/menu/index.vue'),
        meta: { title: '菜单', requireAuth: true, affix: true, closable: false },
      }
    ]
  },
  {
    path: '/user',
    meta: { title: '用户管理' },
    name: "用户管理",
    component: Framework,
    children: [
      {
        path: '/user',
        name: '用户',
        component: () => import('../../views/user/index.vue'),
        meta: { title: '用户' },
      }]
  },
)
```


View Code
**创建数据库表**


 　　根据菜单格式创建数据库表，并创建初始值。


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
CREATE TABLE [dbo].[Sys_Menu](
    [Id] [uniqueidentifier] NOT NULL,
    [Pid] [varchar](50) NOT NULL,
    [CorporationKey] [varchar](50) NOT NULL,
    [SystemKey] [varchar](50) NOT NULL,
    [MenuUrl] [varchar](50) NOT NULL,
    [MenuIcon] [varchar](50) NULL,
    [MenuTitle] [nvarchar](50) NOT NULL,
    [Component] [varchar](500) NOT NULL,
    [Sort] [int] NOT NULL,
    [IsOpen] [bit] NOT NULL,
    [CreateTime] [datetime] NOT NULL,
    [CreateUser] [varchar](50) NOT NULL,
    [RequireAuth] [bit] NULL,
    [Redirect] [varchar](500) NULL,
 CONSTRAINT [PK_Menu] PRIMARY KEY CLUSTERED 
(
    [Id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]
GO
INSERT [dbo].[Sys_Menu] ([Id], [Pid], [CorporationKey], [SystemKey], [MenuUrl], [MenuIcon], [MenuTitle], [Component], [Sort], [IsOpen], [CreateTime], [CreateUser], [RequireAuth], [Redirect]) VALUES (N'380ca40b-8b62-4ebe-86d7-91ae48292f43', N'0', N'6a75ec49-2093-4b89-950f-65e6e72746da', N'6e746ed0-12e9-4002-9887-d84a19142304', N'/panel', N'layui-icon-engine', N'工作空间', N'frameWork', 1, 1, CAST(N'2024-12-05T00:00:00.000' AS DateTime), N'1', 1, NULL)
INSERT [dbo].[Sys_Menu] ([Id], [Pid], [CorporationKey], [SystemKey], [MenuUrl], [MenuIcon], [MenuTitle], [Component], [Sort], [IsOpen], [CreateTime], [CreateUser], [RequireAuth], [Redirect]) VALUES (N'380ca40b-8b62-4ebe-86d7-91ae48292f44', N'380ca40b-8b62-4ebe-86d7-91ae48292f43', N'6a75ec49-2093-4b89-950f-65e6e72746da', N'6e746ed0-12e9-4002-9887-d84a19142304', N'/panel', N'layui-icon-engine', N'工作台', N'../views/panel/index', 1, 1, CAST(N'2024-12-05T00:00:00.000' AS DateTime), N'1', 1, NULL)
INSERT [dbo].[Sys_Menu] ([Id], [Pid], [CorporationKey], [SystemKey], [MenuUrl], [MenuIcon], [MenuTitle], [Component], [Sort], [IsOpen], [CreateTime], [CreateUser], [RequireAuth], [Redirect]) VALUES (N'380ca40b-8b62-4ebe-86d7-91ae48292f45', N'0', N'6a75ec49-2093-4b89-950f-65e6e72746da', N'6e746ed0-12e9-4002-9887-d84a19142304', N'/menu', N'layui-icon-engine', N'菜单管理', N'frameWork', 1, 1, CAST(N'2024-12-05T00:00:00.000' AS DateTime), N'1', 1, NULL)
INSERT [dbo].[Sys_Menu] ([Id], [Pid], [CorporationKey], [SystemKey], [MenuUrl], [MenuIcon], [MenuTitle], [Component], [Sort], [IsOpen], [CreateTime], [CreateUser], [RequireAuth], [Redirect]) VALUES (N'380ca40b-8b62-4ebe-86d7-91ae48292f46', N'380ca40b-8b62-4ebe-86d7-91ae48292f45', N'6a75ec49-2093-4b89-950f-65e6e72746da', N'6e746ed0-12e9-4002-9887-d84a19142304', N'/menu', N'layui-icon-engine', N'菜单', N'../views/menu/index', 1, 1, CAST(N'2024-12-05T00:00:00.000' AS DateTime), N'1', 1, NULL)
INSERT [dbo].[Sys_Menu] ([Id], [Pid], [CorporationKey], [SystemKey], [MenuUrl], [MenuIcon], [MenuTitle], [Component], [Sort], [IsOpen], [CreateTime], [CreateUser], [RequireAuth], [Redirect]) VALUES (N'380ca40b-8b62-4ebe-86d7-91ae48292f47', N'0', N'6a75ec49-2093-4b89-950f-65e6e72746da', N'6e746ed0-12e9-4002-9887-d84a19142304', N'/user', N'layui-icon-engine', N'用户管理', N'frameWork', 1, 1, CAST(N'2024-12-05T00:00:00.000' AS DateTime), N'1', 1, NULL)
INSERT [dbo].[Sys_Menu] ([Id], [Pid], [CorporationKey], [SystemKey], [MenuUrl], [MenuIcon], [MenuTitle], [Component], [Sort], [IsOpen], [CreateTime], [CreateUser], [RequireAuth], [Redirect]) VALUES (N'380ca40b-8b62-4ebe-86d7-91ae48292f48', N'380ca40b-8b62-4ebe-86d7-91ae48292f47', N'6a75ec49-2093-4b89-950f-65e6e72746da', N'6e746ed0-12e9-4002-9887-d84a19142304', N'/user', N'layui-icon-engine', N'用户', N'../views/user/index', 1, 1, CAST(N'2024-12-05T00:00:00.000' AS DateTime), N'1', 1, NULL)
ALTER TABLE [dbo].[Sys_Menu] ADD  CONSTRAINT [DF_Sys_Menu_Sort]  DEFAULT ((0)) FOR [Sort]
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'菜单id' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'Id'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'父级id' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'Pid'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'公司Key' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'CorporationKey'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'系统Key' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'SystemKey'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'菜单路径' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'MenuUrl'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'菜单图标' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'MenuIcon'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'菜单标题' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'MenuTitle'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'排序' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'Sort'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'是否开启菜单' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'IsOpen'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'创建时间' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'CreateTime'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'创建人员' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'CreateUser'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'是否验证' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'RequireAuth'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'重定向' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu', @level2type=N'COLUMN',@level2name=N'Redirect'
GO
EXEC sys.sp_addextendedproperty @name=N'MS_Description', @value=N'菜单表' , @level0type=N'SCHEMA',@level0name=N'dbo', @level1type=N'TABLE',@level1name=N'Sys_Menu'
GO
```


View Code
**编写后端代码**


注意：底层仓储我已经搭建好，只需要实现菜单查询的业务逻辑即可，如果需要了解底层仓储的，请查看《[dapper搭建底层仓储](https://github.com)》


**1：创建模型（我的创建路径：Model\-\>DomainModel\-\>Sys）**


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
/// 
/// 菜单表模型
/// 
public class SysMenu
{
    /// 
    /// 菜单主键
    /// 
    public Guid Id { get; set; }

    /// 
    /// 上级菜单
    /// 
    public string? Pid { get; set; }

    /// 
    /// 公司key
    /// 
    public string? CorporationKey { get; set; }

    /// 
    /// 系统Key
    /// 
    public string? SystemKey { get; set; }

    /// 
    /// 菜单路径
    /// 
    public string? MenuUrl { get; set; }

    /// 
    /// 菜单图标
    /// 
    public string? MenuIcon { get; set; }

    /// 
    /// 菜单标题
    /// 
    public string? MenuTitle { get; set; }

    /// 
    /// 菜单模板
    /// 
    public string? Component { get; set; }

    /// 
    /// 是否开启
    /// 
    public bool IsOpen { get; set; }

    /// 
    /// 排序
    /// 
    public int Sort { get; set; }

    /// 
    /// 创建时间
    /// 
    public DateTime CreateTime { get; set; }

    /// 
    /// 创建人员
    /// 
    public string? CreateUser { get; set; }

    /// 
    /// 是否验证
    /// 
    public bool RequireAuth { get; set; }

    /// 
    /// 重定向目录
    /// 
    public string? Redirect { get; set; }
}
```


View Code
　　**2：创建菜单表的仓储**（我的创建路径：Infrastructure\-\>IRepository和Repository\-\>Sys）****




```
/// 
/// 系统菜单仓储接口
/// 
public interface ISysMenuRepository : IRepository
{
}
```




```
/// 
/// 系统菜单仓储接口实现
/// 
public class SysMenuRepository : Repository, ISysMenuRepository
{
}
```


　　**3：创建领域服务**（我的创建路径：DomainService\-\>IService和Service\-\>Sys）****




```
/// 
/// 菜单服务接口
/// 
public interface ISysMenuService
{
    /// 
    /// 获取树形菜单
    /// 
    /// 
    List GetMenuTreeList();
}
```




```
 /// 
 /// 菜单服务实现
 /// 
 public class SysMenuService : ISysMenuService
 {
     #region 构造实例化

     /// 
     /// 菜单仓储接口
     /// 
     private readonly ISysMenuRepository _menuRepository;

     /// 
     /// 构造函数
     /// 
     /// 
     public SysMenuService(ISysMenuRepository menuRepository)
     {
         _menuRepository = menuRepository;
     }

     #endregion


     #region 业务逻辑

     /// 
     /// 获取树形菜单
     /// 
     /// 
     public List GetMenuTreeList()
     {
          return new List();
     }

     #endregion
 }
```


　　**4：递归获取菜单，呈现上下级关系**


这块主要是把数据库取出的数据，转换成前端能识别的树形结构。要实现该功能，先要建立一个支持树形结构的输出模型（Model\-\>BusinessModel\-\>OutPut）。


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
/// 
/// 菜单输出模型
/// 
public class SysMenuOutPut
{
    /// 
    /// 菜单主键
    /// 
    public Guid Id { get; set; }

    /// 
    /// 上级菜单
    /// 
    public string? Pid { get; set; }

    /// 
    /// 公司key
    /// 
    public string? CorporationKey { get; set; }

    /// 
    /// 系统Key
    /// 
    public string? SystemKey { get; set; }

    /// 
    /// 菜单路径
    /// 
    public string? Path { get; set; }

    /// 
    /// 菜单图标
    /// 
    public string? MenuIcon { get; set; }

    /// 
    /// 菜单标题
    /// 
    public string? Name { get; set; }

    /// 
    /// 菜单模板
    /// 
    public string? Component { get; set; }

    /// 
    /// 是否开启
    /// 
    public bool IsOpen { get; set; }

    /// 
    /// 排序
    /// 
    public int Sort { get; set; }

    /// 
    /// 创建时间
    /// 
    public DateTime CreateTime { get; set; }

    /// 
    /// 创建人员
    /// 
    public string? CreateUser { get; set; }

    /// 
    /// 是否验证
    /// 
    public bool RequireAuth { get; set; }

    /// 
    /// 重定向目录
    /// 
    public string? Redirect { get; set; }

    /// 
    /// 子节点
    /// 
    public List? Children { get; set; }
}
```


View Code
　　然后我们要创建一个菜单的核心操作类，以便系统后续的使用（CoreDomain\-\>BusinessCore）


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
/// 
/// 菜单核心
/// 
public static class MenuCore
{
    /// 
    /// 递归获取菜单,组成树形结构
    /// 
    /// 菜单数据
    /// 返回菜单的树形结构
    public static List GetMenuTreeList(List menuList)
    {
        List list = new();
        List menuListDto = new();
        //模型的转换
        foreach (var item in menuList)
        {
            SysMenuOutPut model = new()
            {
                Id = item.Id,
                Pid = item.Pid,
                CorporationKey = item.CorporationKey,
                SystemKey = item.SystemKey,
                Path = item.MenuUrl,
                Name = item.MenuTitle,
                MenuIcon = item.MenuIcon,
                Component = item.Component,
                IsOpen = item.IsOpen,
                Sort = item.Sort,
                RequireAuth = item.RequireAuth,
                Redirect = item.Redirect,
                CreateTime = item.CreateTime,
                CreateUser = item.CreateUser,
            };
            list.Add(model);
        }
        //递归所有父级菜单
        foreach (var data in list.Where(f => f.Pid == "0" && f.IsOpen))
        {
            var childrenList = GetChildrenMenu(list, data.Id).OrderBy(f => f.Sort).ToList();
            data.Children = childrenList.Count == 0 ? null : childrenList;
            menuListDto.Add(data);
        }
        return menuListDto;
    }

    /// 
    /// 实现递归
    /// 
    /// 菜单数据
    /// 菜单ID
    /// 
    private static List GetChildrenMenu(List moduleOutput, Guid id)
    {
        List sysShowTempMenus = new();
        //得到子菜单
        var info = moduleOutput.Where(w => w.Pid == id.ToString() && w.IsOpen).ToList();
        //循环
        foreach (var sysMenuInfo in info)
        {
            var childrenList = GetChildrenMenu(moduleOutput, sysMenuInfo.Id);
            //把子菜单放到Children集合里
            sysMenuInfo.Children = childrenList.Count == 0 ? null : childrenList;
            //添加父级菜单
            sysShowTempMenus.Add(sysMenuInfo);
        }
        return sysShowTempMenus;
    }
}
```


View Code
　　在领域服务中实现接口GetMenuTreeList();




```
/// 
/// 获取树形菜单
/// 
/// 
public List GetMenuTreeList()
{
    var menuList = _menuRepository.GetAll(BaseSqlRepository.sysMenu_selectAllSql);
    var menuTreeList = MenuCore.GetMenuTreeList(menuList);
    return menuTreeList;
}
```


注意：BaseSqlRepository.sysMenu\_selectAllSql 是查询sql的语句，我放在了一个基础sql仓储中，统一管理


　**5：编写接口（Controllers\-\>Sys）**


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
 /// 
 /// 系统模块
 /// 
 [ApiController]
 [Route("api/[controller]/[action]")]
 [ApiExplorerSettings(GroupName = nameof(ModeuleGroupEnum.SysMenu))]
 public class SysMenuController : BaseController
 {

     #region 构造实列化

     /// 
     /// 菜单服务服务
     /// 
     public ISysMenuService _sysMenuService;

     /// 
     /// 构造函数
     /// 
     /// 
     public SysMenuController(ISysMenuService sysMenuService)
     {
         _sysMenuService = sysMenuService;
     }

     #endregion

     #region 菜单接口

     /// 
     /// 获取树形菜单
     /// 
     /// 
     [HttpGet]
     public ReceiveStatus GetMenuTreeList()
     {
         ReceiveStatus receiveStatus = new();
         var list = _sysMenuService.GetMenuTreeList();
         receiveStatus.data = list;
         return receiveStatus;
     }

     #endregion

 }
```


View Code
　　**6：测试接口**


**![](https://img2024.cnblogs.com/blog/1158526/202412/1158526-20241205162448522-2076632676.png)**


**编写前端代码**


后端接口已经准备就绪，那么接下来就要编写前端的代码，把静态的json数据转换成接口返回的动态数据。


**第一步：修改静态路由**


把base\-routes.ts文件中的路由换成


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
routes.push(
  {
    path: '/framework',
    component: frameWork,
    name: "架构",

  },
  {
    path: '/login',
    component: Login,
    name: "登录页面",
  },
  {
    path: '/panel',
    redirect: '/panel/index',
    meta: { title: '工作空间' },
    name: "工作空间",
    component: frameWork,
    children: [
      {
        path: '/panel',
        name: '工作台',
        component: () => import('../../views/panel/index.vue'),
        meta: { title: '工作台', requireAuth: true, affix: true, closable: false },
      }
    ]
  })
```


View Code
只留下固定的路由菜单，把菜单管理、用户管理等菜单去掉，我们做动态获取。


修改路由守卫如下




```
router.beforeEach((to: RouteLocationNormalized, from: RouteLocationNormalized, next: NavigationGuardNext) => {
  NProgress.start();
  const userStore = useUserStore();
  const endTime = new Date(userStore.expiresDate);
  const currentTime = new Date();
  to.path = to.path;
  if (to.meta.requireAuth && endTime < currentTime) {
    router.push('/login')
  }
  if (to.meta.requireAuth) {
    next();
  } else if (to.matched.length == 0) {
    next({ path: '/panel' })
  } else {
    next();
  }
})
```


这块对比上次的代码，是把next({ path: '/login' })换成了next({ path: '/panel' })。


**第二步：添加接口**


新建文件api\-\>menu\-\>index.ts




```
import Http from '../http';
export const getMenuTreeData = async function() {
    return await Http.get('/api/SysMenu/GetMenuTreeList');
 }
```


该接口是上面我们编写的获取菜单接口


**第三步：递归菜单，并动态添加到路由中**


　　在store\-\>user.ts文件如下，添加如下代码


　　该代码是把后端获取的树形菜单数据，转换成路由能认识的菜单。




```
const defineRouteComponents: Record<string, any> = {
  frameWork: () => import('@/views/frameWork/index.vue')
};
const defineRouteComponentKeys = Object.keys(defineRouteComponents);
export const setMenuData = (
  routeMap: any[],
) => {
  return routeMap
    .map(item => {
      const pathArray = item.component.split('/');
      const url = ref();
      if (pathArray.length > 0) {
        if (pathArray.length === 3)
          url.value = import(`../${pathArray[1]}/${pathArray[2]}.vue`);
        if (pathArray.length === 4)
          url.value = import(`../${pathArray[1]}/${pathArray[2]}/${pathArray[3]}.vue`);
        if (pathArray.length === 5)
          url.value = import(`../${pathArray[1]}/${pathArray[2]}/${pathArray[3]}/${pathArray[4]}.vue`);
      };
      const { name, requireAuth, id } = item || {};
      const currentRouter: RouteRecordRaw = {
        // 如果路由设置了 path，则作为默认 path，否则 路由地址 动态拼接生成如 /dashboard/workplace
        path: item.path,
        // 路由名称，建议唯一
        //name: `${item.id}`,
        // meta: 页面标题, 菜单图标, 页面权限(供指令权限用，可去掉)
        meta: {
          name,
          requireAuth,
          id
        },
        name: item.name,
        children: [],
        // 该路由对应页面的 组件 (动态加载 @/views/ 下面的路径文件)
        component: item.component && defineRouteComponentKeys.includes(item.component)
          ? defineRouteComponents[item.component]
          : () => url.value,

      };

      // 为了防止出现后端返回结果不规范，处理有可能出现拼接出两个 反斜杠
      if (!currentRouter.path.startsWith('http')) {
        currentRouter.path = currentRouter.path.replace('//', '/');
      }

      // 重定向
      item.redirect && (currentRouter.redirect = item.redirect);
      if (item.children != null) {
        // 子菜单，递归处理
        currentRouter.children = setMenuData(item.children);
      }
      if (currentRouter.children === undefined || currentRouter.children.length <= 0) {
        currentRouter.children;
      }
      return currentRouter;
    })
    .filter(item => item);
};
```


然后在defineStore的actions中添加如下方法




```
  actions: {
    //获取菜单数据，并递归实现动态路由菜单
    async loadMenus() {
      new Promise(async (resolve, reject) => {
        const { data, code, msg } = await getMenuTreeData();
        if (code == 200) {
          this.menus = data;
          var menuList = setMenuData(data) as RouteRecordRaw[]
          menuList.map(d => {
            router.addRoute(d);
          })
          resolve(menuList);
        }
        else {
          this.menus = [];
          ElMessage({
            message: msg,
            type: "error",
          });
        }
      });
    },
  },
```


user.ts完整代码如下（动态路由菜单的核心）


![](https://images.cnblogs.com/OutliningIndicators/ContractedBlock.gif)![](https://images.cnblogs.com/OutliningIndicators/ExpandedBlockStart.gif)


```
import { getMenuTreeData } from '@/api/menu';
import router from '@/router';
import { ElMessage } from 'element-plus';
import { defineStore } from 'pinia'
import { ref } from 'vue';
import { RouteRecordRaw } from 'vue-router';
export const useUserStore = defineStore(
  'user', {
  state: () => ({
    token: '',
    expiresDate: '',
    userInfo: {},
    menus: [] as any,
  }),

  actions: {
    //获取菜单数据，并递归实现动态路由菜单
    async loadMenus() {
      new Promise(async (resolve, reject) => {
        const { data, code, msg } = await getMenuTreeData();
        if (code == 200) {
          this.menus = data;
          var menuList = setMenuData(data) as RouteRecordRaw[]
          menuList.map(d => {
            router.addRoute(d);
          })
          resolve(menuList);
        }
        else {
          this.menus = [];
          ElMessage({
            message: msg,
            type: "error",
          });
        }
      });
    },
  },
  persist: {
    enabled: true,
    strategies: [
      {
        // 可以是localStorage或sessionStorage
        storage: localStorage,
        // 指定需要持久化的属性
        paths: ['token', 'expiresDate', 'userInfo', 'menus']
      }
    ]
  },
})


const defineRouteComponents: Record<string, any> = {
  frameWork: () => import('@/views/frameWork/index.vue')
};
const defineRouteComponentKeys = Object.keys(defineRouteComponents);
export const setMenuData = (
  routeMap: any[],
) => {
  return routeMap
    .map(item => {
      const pathArray = item.component.split('/');
      const url = ref();
      if (pathArray.length > 0) {
        if (pathArray.length === 3)
          url.value = import(`../${pathArray[1]}/${pathArray[2]}.vue`);
        if (pathArray.length === 4)
          url.value = import(`../${pathArray[1]}/${pathArray[2]}/${pathArray[3]}.vue`);
        if (pathArray.length === 5)
          url.value = import(`../${pathArray[1]}/${pathArray[2]}/${pathArray[3]}/${pathArray[4]}.vue`);
      };
      const { name, requireAuth, id } = item || {};
      const currentRouter: RouteRecordRaw = {
        // 如果路由设置了 path，则作为默认 path，否则 路由地址 动态拼接生成如 /dashboard/workplace
        path: item.path,
        // 路由名称，建议唯一
        //name: `${item.id}`,
        // meta: 页面标题, 菜单图标, 页面权限(供指令权限用，可去掉)
        meta: {
          name,
          requireAuth,
          id
        },
        name: item.name,
        children: [],
        // 该路由对应页面的 组件 (动态加载 @/views/ 下面的路径文件)
        component: item.component && defineRouteComponentKeys.includes(item.component)
          ? defineRouteComponents[item.component]
          : () => url.value,

      };

      // 为了防止出现后端返回结果不规范，处理有可能出现拼接出两个 反斜杠
      if (!currentRouter.path.startsWith('http')) {
        currentRouter.path = currentRouter.path.replace('//', '/');
      }

      // 重定向
      item.redirect && (currentRouter.redirect = item.redirect);
      if (item.children != null) {
        // 子菜单，递归处理
        currentRouter.children = setMenuData(item.children);
      }
      if (currentRouter.children === undefined || currentRouter.children.length <= 0) {
        currentRouter.children;
      }
      return currentRouter;
    })
    .filter(item => item);
};
```


View Code
编写完以上代码，我们已经获取到后端的菜单，并且已添加到动态路由中。接下来只需要在登录时，调用loadMenus()方法即可。


**第四步：登录后获取动态路由菜单**


如图


![](https://img2024.cnblogs.com/blog/1158526/202412/1158526-20241210152649123-1840775499.png)


**第五步：传入token**


　　当你辛苦完成以上步骤后，你迫不及待的想查看下效果。但是系统给你泼了一盆冷水，提示接口401错误。


没错，会出现错误，因为我们系统使用了jwt鉴权，所以我们需要把token传给后端，然后进行验证。只有通过后才能访问接口。


那么要如何做才能把token传给后端呢。


在我们之前写好的请求拦截中，加入如下代码


ps：不清楚请求拦截的，请观看[（系列十一）Vue3框架中路由守卫及请求拦截（实现前后端交互）](https://github.com)




```
 /* 请求拦截 */
        this.service.interceptors.request.use((config: InternalAxiosRequestConfig) => {
            //可以在这里做请求拦截处理   如：请求接口前，需要传入的token
            const userInfoStore = useUserStore();
            if (userInfoStore.token) {
                (config.headers as AxiosRequestHeaders).token = userInfoStore.token as string
                config.headers["Authorization"] = "Bearer " + userInfoStore.token;
            } else {
                if (router.currentRoute.value.path !== '/login') {
                    router.push('/login');
                }
            }
            return config
        }, (error: any) => {
            ElMessage({
                message: "接口调用失败",
                type: "error",
            });
            return error.message;
            //return Promise.reject(error);
        })
```


然后运行项目，你会发现，你的菜单实现了动态路由，全部由数据库获取。


以上就是本篇文章的全部内容，感谢耐心观看


****后端WebApi**预览地址：http://139\.155\.137\.144:8880/swagger/index.html**


**前端vue 预览地址：http://139\.155\.137\.144:8881**


**关注公众号：发送【权限】，获取前后端代码**


**有兴趣的朋友，请关注我微信公众号吧(\*^▽^\*)。**


**![](https://img2024.cnblogs.com/blog/1158526/202408/1158526-20240824140446786-404771438.png)**


关注我：一个全栈多端的宝藏博主，定时分享技术文章，不定时分享开源项目。关注我，带你认识不一样的程序世界



# eShopOnWeb 微软

微软官方教程，示例 Asp.Net Core 参考应用程序, 由微软提供支持, 演示单进程 (单片) 应用程序体系结构和部署模型。 

此参考应用程序可以免费下载电子书: [Architecting Modern Web Applications with ASP.NET Core and Azure](https://aka.ms/webappebook), 目前更新为 **ASP.NET Core 2.1**。

你也可以阅读在线网页上阅读该电子书 .Net docs地址：
https://docs.microsoft.com/en-us/dotnet/standard/modern-web-apps-azure-architecture/

![image](https://user-images.githubusercontent.com/1712635/42467632-449688c2-8367-11e8-9323-81ab50a66006.png)

** ecoconweb ** 示例与 [eShopOnContainers](https://github.com/dotnet/eShopOnContainers) 示例应用程序有关, 在这种情况下, 该应用程序侧重于基于微服务/容器的应用程序体系结构。但是, **eShopOnWeb** 在当前功能方面要简单得多, 并且专注于具有单个部署的传统 web 应用程序开发。

本示例的目的是演示 [eBook](https://aka.ms/webappebook) 中描述的一些原则和模式。它并不意味着就是一个电子商务参考应用程序, 因此它没有实现许多功能, 这将是显而易见的, 或必不可少的一个真正的电子商务应用程序。

> ### 版本
> #### 这个 `master` 分支 当前运行的是 ASP.NET Core 2.1。
> #### 旧版本也被标记(可查看其他分支)。

## 主题 (电子书目录)

-简介
-现代 web 应用程序的特性
-在传统 web 应用和 SPA（单页面应用程序） 之间进行选择
-建筑原则
-常见的 web 应用程序体系结构
-通用客户端技术
-开发 ASP.NET Core MVC 应用程序
-使用 ASP.NET Core 应用程序中的数据
-测试 ASP.NET Core MVC 应用程序
-目标托管 ASP.NET Core 应用程序的开发过程
-ASP.NET Core Web 应用程序的 azure 托管建议

## 运行示例

克隆或下载示例后, 您应该能够立即使用内存中数据库运行它。

如果您希望将该示例与持久数据库一起使用, 则需要先运行其Entity Framework Core迁移, 然后才能运行该应用程序, 并更新 "startup. cs" 中的 "ConfigureServices" 方法 (见下文)。

您还可以在 docker 中运行示例 (见下文)。

### 将示例配置为使用 SQL server 服务器

1. 更新 `Startup.cs`的 `ConfigureDevelopmentServices` 方法，按照下边的方法:

```
        public void ConfigureDevelopmentServices(IServiceCollection services)
        {
            // 使用内存数据库
            //ConfigureTestingServices(services);

            // 使用真实数据库
            ConfigureProductionServices(services);

        }
```

1. 确保连接字符串在 "appationts. json" 中指向本地 sql server 实例。

2. 在 web 文件夹中打开命令提示符并执行以下命令:

```
dotnet restore
dotnet ef database update -c catalogcontext -p ../Infrastructure/Infrastructure.csproj -s Web.csproj
dotnet ef database update -c appidentitydbcontext -p ../Infrastructure/Infrastructure.csproj -s Web.csproj
```

这些命令将创建两个单独的数据库, 一个用于商店的目录数据和购物车信息, 另一个用于应用的用户凭据和标识数据。

3. 运行应用程序。
第一次运行应用程序时, 它将使用数据为两个数据库添加种子, 以便您可以在存储区中看到产品, 并且您应该能够使用 demouser@microsoft.com 帐户登录。

注意: 如果需要创建迁移, 可以使用以下命令:
```
-- create migration (from Web folder CLI)
dotnet ef migrations add InitialModel --context catalogcontext -p ../Infrastructure/Infrastructure.csproj -s Web.csproj -o Data/Migrations

dotnet ef migrations add InitialIdentityModel --context appidentitydbcontext -p ../Infrastructure/Infrastructure.csproj -s Web.csproj -o Identity/Migrations
```

## 使用 docker 运行示例

通过从根文件夹 (. sln 文件所在的位置) 运行这些命令, 可以同时运行 web 和 webrazorpages 示例:

```
    docker-compose build
    docker-compose up
```

一旦这些命令完成, 您应该能够向 localhost:5106 和 localhost:5107 发出请求。

通过使用位于项目根目录中各自的 "dokerfile" 文件中的说明, 您可以只运行 web 或 webrazorpages 应用程序。同样, 从解决方案的根目录 (. sln 文件所在的位置) 运行这些命令。

> nuget包服务器方便我们团队管理项目库资源，解决了团队中项目库cope引用繁琐的问题
> 

- 以下步骤来简单描述如何搭建自己的nuget包服务器

---
## 一、使用VS新建一个asp.net Web项目
> 这里就简单的说明下，相必VS开发者都知道怎么建立一个web项目
## 二、通过Nuget包管理引用Nuget.Server 
>建立好asp.net web项目后在项目中点击右键->管理Nuget程序包->搜索Nuget.Server 选择你适合的版本安装到项目中即可
## 三、配置相关appKey

配置文件如下：


```
<?xml version="1.0" encoding="utf-8"?>
<!--
  For more information on how to configure your ASP.NET application, please visit
  http://go.microsoft.com/fwlink/?LinkId=169433
  -->
<configuration>
  <appSettings>
    <!--
    决定是否需要推动\ Api密钥从服务器删除软件包。 
    -->
    <add key="requireApiKey" value="true" />
    
    <!-- 
    将值设置为允许人们将从服务器/删除软件包。 注意:这是一个共享密钥(密码)为所有用户。
    设置这个后就可以通过nuget package exploer来发布自己的项目包到自己的nuget服务器中
    -->
    <add key="apiKey" value="123456" />
    
    <!--
    改变包文件夹的路径。默认是~ /package。
    -->
    <add key="packagesPath" value="" />

    <!--
    Set allowOverrideExistingPackageOnPush to false to mimic NuGet.org's behaviour (do not allow overwriting packages with same id + version).
    -->
    <add key="allowOverrideExistingPackageOnPush" value="false" />

    <!--
    Set ignoreSymbolsPackages to true to filter out symbols packages. Since NuGet.Server does not come with a symbol server,
    it makes sense to ignore this type of packages. When enabled, files named `.symbols.nupkg` or packages containing a `/src` folder will be ignored.
    
    If you only push .symbols.nupkg packages, set this to false so that packages can be uploaded.
    -->
    <add key="ignoreSymbolsPackages" value="true" />
    
    <!--
    Set enableDelisting to true to enable delist instead of delete as a result of a "nuget delete" command.
    - delete: package is deleted from the repository's local filesystem.
    - delist: 
      - "nuget delete": the "hidden" file attribute of the corresponding nupkg on the repository local filesystem is turned on instead of deleting the file.
      - "nuget list" skips delisted packages, i.e. those that have the hidden attribute set on their nupkg.
      - "nuget install packageid -version version" command will succeed for both listed and delisted packages.
        e.g. delisted packages can still be downloaded by clients that explicitly specify their version.
    -->
    <add key="enableDelisting" value="false" />

    <!--
    Set enableFrameworkFiltering to true to enable filtering packages by their supported frameworks during search.
    -->
    <add key="enableFrameworkFiltering" value="false" />
    
    <!--
    When running NuGet.Server in a NAT network, ASP.NET may embed the erver's internal IP address in the V2 feed.
    Uncomment the following configuration entry to enable NAT support.
    -->
    <!-- <add key="aspnet:UseHostHeaderForRequestUrl" value="true" /> -->
  </appSettings>
</configuration>
```
以上配置文件这里只展示了主要的几个配置项，其他的没展示出来，实际以引用Nuget.Server包后生成的web.config为准

## 四、发布

 这里发布需要使用到NuGet package Exploer 发布工具，这里就不一一说明，以下我说明一下一个解决坑的地方
>  当你服务器上的IIS安装了WebDev 后，可能会出现发布包发布不了的问题，会出现method not allowed 出现这个原因是由于安装了WebDev 导致发布包的方式默认是Put方式导致的 

让我们一起进入Nuget的便捷之旅吧
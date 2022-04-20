在 POM 中，groupId, artifactId, packaging, version 叫作 maven 坐标，它能唯一的确定一个项目。有了 maven 坐标，我们就可以用它来指定我们的项目所依赖的其他项目，插件，或者父项目。一般 maven 坐标写成如下的格式：  
groupId:artifactId:packaging:version


在 POM 的 dependencies 部分中，scope 决定了依赖关系的适用范围。控制哪些依赖在哪些classpath 中可用，哪些依赖包含在一个应用中。

-   **compile** **（编译）**

compile是默认的范围；如果没有提供一个范围，那该依赖的范围就是编译范围。编译范围依赖在所有的classpath中可用，同时它们也会被打包。

-   **provided** **（已提供）**

provided 依赖只有在当JDK 或者一个容器已提供该依赖之后才使用。例如， 如果你开发了一个web 应用，你可能在编译 classpath 中需要可用的Servlet API 来编译一个servlet，但是你不会想要在打包好的WAR 中包含这个Servlet API；这个Servlet API JAR 由你的应用服务器或者servlet 容器提供。已提供范围的依赖在编译classpath （不是运行时）可用。它们不是传递性的，也不会被打包。

-   **runtime** **（运行时）**

runtime 依赖在运行和测试系统的时候需要，但在编译的时候不需要。比如，你可能在编译的时候只需要JDBC API JAR，而只有在运行的时候才需要JDBC  
驱动实现。

-   **test** **（测试）**

test范围依赖 在一般的编译和运行时都不需要，它们只有在测试编译和测试运行阶段可用。

-   **system** **（系统）**

system范围依赖与provided类似，但是你必须显式的提供一个对于本地系统中JAR文件的路径。这么做是为了允许基于本地对象编译，而这些对象是系统类库的一部分。这样的构建应该是一直可用的，Maven 也不会在仓库中去寻找它。如果你将一个依赖范围设置成系统范围，你必须同时提供一个**systemPath**元素。注意该范围是不推荐使用的（建议尽量去从公共或定制的 Maven 仓库中引用依赖）。

-   **import(导入)**

import仅支持在`<**dependencyManagement**>`中的类型依赖项上。它表示要在指定的POM `<**dependencyManagement**>`部分中用有效的依赖关系列表替换的依赖关系。该scope类型的依赖项实际上不会参与限制依赖项的可传递性。
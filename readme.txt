# module path(模块路径详解)
1.版本仓库根路径(github.com/video)
仓库名：github.com/winnerxue/video

2.模块路径(通常为空,可以指定在仓库目录下创建模块,不限一级目录)
仓库名：github.com/winnerxue/video
模块路径1：github.com/winnerxue/video/image
模块路径2：github.com/winnerxue/video/image/aa

3.v2,v3,vn(主版本号在2以及以上的代码仓库,模块路径目录必须以v2,v3,vn结尾)，在打tag时根据v2的提交去打v2.x.x
仓库名：github.com/winnerxue/video
V2版本仓库名为：github.com/winnerxue/video/v2

4.可以使用同一个代码仓库的不同分支作为版本之间的开发(因为两个分支已经是不兼容的)
仓库名：github.com/winnerxue
分支名v2,模块路径为:github.com/winnerxue/yaml.v2
分支名v3,模块路径为:github.com/winnerxue/yaml.v3

# version(版本号详解)
1.版本号对应模块的一个以v开头的tag用于标识某个release或pre-release（主版本.次版本.补丁版本-预发布前缀+构建元信息）
v1.2.3-beta+20221010235634-sfjoweiaabdfa

2.主版本号<0表示模块还不稳定,版本不做兼容性考虑,主版本号>=2表示模块不兼容主版本1和主版本0
  次版本号之间应该做兼容,主要是开发迭代功能,补丁版本主要用于本次功能迭代修复问题

3.伪版本(由于依赖模块的代码仓库不存在任何tag标签,默认拉取主干分支最后一次commit的版本
  非v0.0.0的伪版本是因为当开发阶段测试功能,并没有打tag时使用)
v0.0.0-20220910234518-kdjfafadfadf
v1.0.0-20220910234518-fdsafkdlajfl
  (可以通过以下命令下载模块并自动填写伪版本号)
go get github.com/winnerxue/yaml.v2@master
go get github.com/winnerxue/yaml.v2@v2.1
go get github.com/winnerxue/yaml.v2@adskfjlasdjfladf


# replace详解(当自定义第三方类库是使用：先fock仓库,使用replace将三方库替代,也可以不fock库,直接clone到本地相对目录使用)
(1).fock到可访问或想自定义的仓库
(2).replace github.com/dafd/gin v1.0.1 => github.com/winnerxue/gin v1.0.1（只替代v1.0.1版本）
(3).replace github.com/dafd/gin => gitub.com/winnerxue/gin（替代所有的版本）
(4).replace github.com/dafd/gin => github.com/winnerxue/gin v1.0.1（只使用v1.0.1版本）【版本强制】
(5).replace github.com/dafd/gin v1.2.3 => github.com/winnerxue/gin v1.0.1（使用v1.0.1替代v1.2.3）

# exclude详解(当依赖库链上有使用到指定版本的时候,会跳过该版本,A项目中有用到P项目的v1.0.0,A依赖于B项目,B依赖于P项目中的v1.0.1,如果不exclude的话,A项目中的go.mod会使用P项目的v1.0.1版本,即版本提升)
(1).exclude github.com/dafd/gin v1.2.3 (当存在比v1.2.3版本要高的版本时,go mod会在版本提升时跳过该版本,选择v1.2.4版本,若不存在则报错)

# retract详解(当某个版本tag存在严重bug时,此时可以删除tag,修改后再打一个相同的tag版本,但是GOModules代理或开发者等上游并不知道,可以标记该版本存在问题)
retract (
  v1.0.0
)

# incompatible在go.mod中兼容性问题
在GoModules出现之前，某些Go框架以及存在v2及以上版本，这些框架没有go.mod，因为如果要使用则会有incompatible标记为不兼容
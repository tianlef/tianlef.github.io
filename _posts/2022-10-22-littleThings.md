# 技术汇总

## 将java文件转变为Maven文件

- IDEA 右键项目
- 点击 Add Framework Support
- 选择Maven

##  Maven 

- groupId
  - 两段
  - 第一段为域
  - 第二段为公司名称
  - org
  - tinnie
- Artifact ID
  - 你创建的应用名称
  - RoutineCat

## Spring Parent 
- 可以提供不同插件之间的最佳依赖
- 可以通过`<properties></properties>`覆盖内部的依赖
  
## Spring-boot-dependencies

- 可以不使用parent
- 自己配置依赖
- 需要在dependencies之前配置需要以来的东西

## IDEA package 展开

- setting
- Tree Appearence
- compact Middle Packages
- 取消

## IDEA配置git路径

- 查询本机git安装路径
  - where git
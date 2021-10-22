## git项目配置

### .gitattributes 文件

```
*.css linguist-language=html
*.less linguist-language=html
*.js linguist-language=html
*.html linguist-language=html
```

这个文件是在github项目中，显示为什么语言类型的项目。

github一般是根据项目中什么文件数目最多来显示项目类型的。如果需要修改，就要加上这个文件。



> 深入一点的话，有个 linguist-language库，可以用于多方位展示language百分比，生成语言分解图。
>
> [git库地址](https://github.com/github/linguist#overrides)

![image-20211015084800071](../assets/img/study/git1.png)

### .gitignore

```
# 注释内容
node_module/
./static
package-lock.json
```

该文件用于忽略提交代码的文件路径or文件
# Jedis 文档

本文档使用 [MkDocs](https://www.mkdocs.org/) 生成静态站点。

请参阅 [mkdocs.yml](../mkdocs.yml) 了解配置。

要在本地开发文档，您可以使用包含的 [Dockerfile](Dockerfile) 构建一个包含所有依赖项的容器，并运行它来预览更改：

```bash
# 在 docs/ 中
docker build -t squidfunk/mkdocs-material .
# cd ..
docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material
```
# Learn_Tinify_Image_Action

学习如何使用TinePNG搭配工作流，实现自动压缩上传的图片。 

Learn how to use TinePNG with workflow to automatically compress uploaded images.

### 步骤

#### 申请 Tinify API key

申请地址：https://tinypng.com/developers

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222141122.png)

在你的邮箱中点击链接登录后，选择 `Add API key`，即可生成。

请记住此token

#### 申请 GitHub Token

在GItHub中点击头像，选择 `Settings`

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222141406.png)

选择`Developer settings`

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222141555.png)

选择 `Personal access tokens` -> `Generate new token`

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222141740.png)

按照下图勾选即可

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/22-12-2021_142047_github.com.jpeg)

请记住此token

#### 配置yaml

在GitHub项目中创建GitHub Action 所需的yaml配置文件，路径为 `/.github/workflows/`，文件名为 `main.yml`

```yaml
name: image
on:
  push:
    paths:
      - 'static/**'
jobs:
  compress:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          ref: ${{ github.head_ref }}
      - uses: namoscato/action-tinify@v1
        with:
          api_key: ${{ secrets.API_KEY }}
          github_token: ${{ secrets.MYGITHUB_TOKEN }}
```

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222112648.png)

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222113333.png)

解释：

* ` 'static/**'`：声明了一旦在`static`文件夹下发生了 push 操作就会触发这个事件
* ` ${{ github.head_ref }}`：分支，GitHub默认环境变量
* `${{ secrets.API_KEY }}`：Tinify API key，Tinify 密钥
* `${{ secrets.MYGITHUB_TOKEN }}`：GitHub 密钥

其他变量详见：https://github.com/marketplace/actions/tinify-image-action

#### 配置环境变量

在项目中选择`Settings` -> `Secrets` -> `New repository secret`

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222140008.png)

<br/>

在`Name`中输入 `API_KEY`，在`Value`中输入你申请的`Tinify API key`

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222142355.png)

<br/>

再次添加，在`Name`中输入 `MYGITHUB_TOKEN`，在`Value`中输入你申请的` GitHub Token`

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222142616.png)

<br/>

最终结果如下

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222142700.png)

<br/>

#### 测试

我们上传一张图片，原图大小为`739.2 kB`

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222143038.png)

<br/>

在`Action`中可以观察到工作流

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222143222.png)

<br/>

此处有详细的工作流执行步骤，此处不多解释。

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222143325.png)

<br/>

再次查看文件大小，已压缩至`585KB`

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222143429.png)

<br/>

在Tinify中能够看到已消耗配额1张

![](https://cdn.jsdelivr.net/gh/leopold7/CDN2@main/static/images/begs/2021/12/20211222143805.png)

### 注意事项与优化

1. Tinify每月500张图片免费压缩，如果需求量大建议支持下Tinify。

2. 如果想baipiao的话（=，= ），可以优化工作流（申请多个key，负载均衡）。
3. 如果我们不想覆盖压缩，可以在压缩前执行备份操作。

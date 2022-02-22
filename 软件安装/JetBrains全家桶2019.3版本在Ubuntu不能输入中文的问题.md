### 【软件安装】JetBrains全家桶2019.3版本在Ubuntu不能输入中文的问题
- 我的Pycharm版本是2019.3，Ubuntu 18.04 LTS，如下。从2019.2版本升级到2019.3版本之后在编辑器内就不能输入中文了，在 bin/pycharm.sh 里面设置了输入法也不起作用。

  ![1](https://user-images.githubusercontent.com/39394831/155042976-6f7cab77-0421-468a-a200-4f4c8f7b6891.png)
- 在菜单栏 Help -> Edit Custom VM Options 添加配置：`-Dauto.disable.input.methods=false`

  ![2](https://user-images.githubusercontent.com/39394831/155043067-4eb53867-caed-42f1-8c49-342c5a2cc686.png)
- 我用的搜狗输入法，正常了。另外，WebStorm 和 IDEA 同样亲测有效。
  
  ![3](https://user-images.githubusercontent.com/39394831/155043129-01cf24cd-0052-452f-b8ae-69a3cf3634df.png)
  ![4](https://user-images.githubusercontent.com/39394831/155043149-6ea50683-089a-443f-8f49-37643412e918.png)

  


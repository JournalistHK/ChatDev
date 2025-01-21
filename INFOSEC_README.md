## **基本信息**

| 条目           | 内容                                                         |
| -------------- | ------------------------------------------------------------ |
| 项目说明       | 以清华的开源项目[ChatDev](https://github.com/OpenBMB/ChatDev)为基础，了解AIAgent的开发思路，理解代码基本运行逻辑，搞明白其代码架构，然后进行调整，从而搭建自己想要的Agnet |
| 项目地址       | [InfoSec](https://gitea.gsealy.net/dengxd/ChatDev)           |
| 开发及维护人员 | 邓旭东                                                       |



## v0.0.0.1

**说明**

将ChatDev源码clone下来后进行调试修改，现可调用公司的大模型，调试配置见`./vscode/launch.json`。
如使用命令行运行，需要先配置`BASE_URL`和`OPENAI_API_KEY`环境变量，运行时再传入`--model`，具体见README.md。

**what's new?**

1. 添加可调用模型参数，通义千问的模型（`qwen-max`和`qwen-coder-plus`）和公司模型（`internlm2.5-chat`）
2. token的encoding模式固定为`gpt-4-turbo`
3. `camel/config.py`中的`logit_bias`参数在公司大模型调用时无法识别，会导致报错，所以暂时注掉



## ”可能“会存在的环境和配置问题

**问题1：配置完成后，运行时报错`TypeError: __init__() got an unexpected keyword argument 'proxies'`**

按照原项目给出步骤进行安装配置，因为openai包没指定httpx包版本，`pip3 install -r requirements.txt`时会自动安装httpx 0.28.* 版本的包导致报错，具体如下：

```
Traceback (most recent call last):
  File "/home/infosec/anaconda3/envs/chatdev/lib/python3.9/runpy.py", line 197, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "/home/infosec/anaconda3/envs/chatdev/lib/python3.9/runpy.py", line 87, in _run_code
    exec(code, run_globals)
  File "/home/infosec/.vscode-server/extensions/ms-python.debugpy-2024.14.0/bundled/libs/debugpy/adapter/../../debugpy/launcher/../../debugpy/__main__.py", line 71, in <module>
    cli.main()
  File "/home/infosec/.vscode-server/extensions/ms-python.debugpy-2024.14.0/bundled/libs/debugpy/adapter/../../debugpy/launcher/../../debugpy/../debugpy/server/cli.py", line 501, in main
    run()
  File "/home/infosec/.vscode-server/extensions/ms-python.debugpy-2024.14.0/bundled/libs/debugpy/adapter/../../debugpy/launcher/../../debugpy/../debugpy/server/cli.py", line 351, in run_file
    runpy.run_path(target, run_name="__main__")
  File "/home/infosec/.vscode-server/extensions/ms-python.debugpy-2024.14.0/bundled/libs/debugpy/_vendored/pydevd/_pydevd_bundle/pydevd_runpy.py", line 310, in run_path
    return _run_module_code(code, init_globals, run_name, pkg_name=pkg_name, script_name=fname)
  File "/home/infosec/.vscode-server/extensions/ms-python.debugpy-2024.14.0/bundled/libs/debugpy/_vendored/pydevd/_pydevd_bundle/pydevd_runpy.py", line 127, in _run_module_code
    _run_code(code, mod_globals, init_globals, mod_name, mod_spec, pkg_name, script_name)
  File "/home/infosec/.vscode-server/extensions/ms-python.debugpy-2024.14.0/bundled/libs/debugpy/_vendored/pydevd/_pydevd_bundle/pydevd_runpy.py", line 118, in _run_code
    exec(code, run_globals)
  File "/home/infosec/dxd/AIAgent/ChatDev/run.py", line 24, in <module>
    from chatdev.chat_chain import ChatChain
  File "/home/infosec/dxd/AIAgent/ChatDev/chatdev/chat_chain.py", line 14, in <module>
    from camel.web_spider import modal_trans
  File "/home/infosec/dxd/AIAgent/ChatDev/camel/web_spider.py", line 13, in <module>
    client = openai.OpenAI(
  File "/home/infosec/anaconda3/envs/chatdev/lib/python3.9/site-packages/openai/_client.py", line 123, in __init__
    super().__init__(
  File "/home/infosec/anaconda3/envs/chatdev/lib/python3.9/site-packages/openai/_base_client.py", line 847, in __init__
    self._client = http_client or SyncHttpxClientWrapper(
  File "/home/infosec/anaconda3/envs/chatdev/lib/python3.9/site-packages/openai/_base_client.py", line 745, in __init__
    super().__init__(**kwargs)
TypeError: __init__() got an unexpected keyword argument 'proxies'
```

**解决方法**

指定httpx包版本为0.27.*（我这里用的是0.27.0）

```
pip3 install httpx==0.27.0
```

也可以在`requirements.txt`中指定httpx的版本


---

**问题2：还没遇到，待补充**
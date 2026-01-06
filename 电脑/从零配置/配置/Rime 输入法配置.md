# Rime 输入法配置

macOS 下 Rime 输入法（鼠须管）的安装与配置指南。

---

## 1. 安装鼠须管

从官网下载安装包：[鼠须管官网](https://rime.im/download/)

安装完成后需**注销或重启**用户会话才能生效。

**配置文件位置**：`~/Library/Rime/`

## 2. 安装薄荷输入法方案

项目地址：[oh-my-rime](https://github.com/Mintimate/oh-my-rime)

**安装步骤**：
1. 下载整个项目（Code → Download ZIP）
2. 解压后将所有文件复制到 `~/Library/Rime/`
3. 点击菜单栏输入法图标 → **重新部署**

## 3. 安装主题

项目地址：[blue-reverie-rime-theme](https://github.com/hunter-ji/blue-reverie-rime-theme)

**安装步骤**：
1. 下载 `squirrel.yaml` 文件
2. 复制到 `~/Library/Rime/`
3. 重新部署

## 4. Rime 配置助手

项目地址：[rime-mate](https://github.com/hunter-ji/rime-mate)

**使用方法**：
1. 下载并解压
2. 双击 `Rime配置助手.command` 运行
3. 按提示安装语言模型

---

## 常用操作

| 操作 | 方法 |
|------|------|
| 重新部署 | 菜单栏图标 → 重新部署 |
| 切换输入方案 | `Ctrl + ~` 或 `F4` |
| 中英切换 | `Shift` |
| 全角/半角 | `Shift + Space` |
| 打开配置目录 | 菜单栏图标 → 用户设定 |

## 自定义配置

在 `~/Library/Rime/` 下创建 `default.custom.yaml` 进行个性化配置：

```yaml
patch:
  # 候选词数量
  menu/page_size: 9

  # 输入方案列表
  schema_list:
    - schema: rime_mint        # 薄荷拼音
    - schema: double_pinyin    # 双拼
```

修改配置后需**重新部署**才能生效。

# Typora + PicGo + 阿里云 OSS 图床配置指南

## 一、软件准备

1. **下载工具**：

   - [Typora 下载](https://www.typora.net/)
   - [PicGo 下载](https://github.com/Molunerfinn/PicGo/releases)

2. **Mac 系统权限问题处理**：
    若打开 PicGo 提示「软件已损坏」，执行以下命令：

   ```
   sudo xattr -d com.apple.quarantine "/Applications/PicGo.app"
   ```

## 二、阿里云 OSS 配置

### 1. 创建 Bucket

1. 登录 [阿里云控制台](https://www.aliyun.com/)，搜索并进入 **OSS 管理**

2. 点击 **创建 Bucket**，按需填写参数：

   | 参数            | 配置说明                                                 |
   | --------------- | -------------------------------------------------------- |
   | **Bucket 名称** | 自定义（如 `my-typora-images`）                          |
   | **地域**        | 就近选择（如 `华东1-杭州`）                              |
   | **存储类型**    | 标准存储                                                 |
   | **读写权限**    | **公共读**（需手动关闭 ACL 阻止,和关闭公共访问限制一样） |

   ![创建 Bucket 示例](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250303194625628.png)

3. **关闭公共访问限制**（创建后操作）：

   - 进入 Bucket → **权限管理** → **公共访问设置** → 关闭 **禁止公共访问**

### 2. 创建文件目录

1. 进入 Bucket → **文件管理** → **新建目录**
2. 输入目录名（如 `md-images`），用于存储 Typora 图片


### 3. 获取 AccessKey

1. 进入 [AccessKey 管理](https://ram.console.aliyun.com/manage/ak)![image-20250303195237587](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250303195237587.png)
2. 创建 AccessKey，保存以下信息：
   - **AccessKey ID**
   - **AccessKey Secret**

![image-20250303200249784](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250303200249784.png)

## 三、PicGo 配置

### 1. 安装插件

1. 打开 PicGo → **插件设置** → 找到 `aliyun`

![image-20250303195526872](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250303195526872.png)



### 2. 配置阿里云图床

1. 进入 **图床设置** → **阿里云OSS** → 填写参数：

   | 参数          | 值                                        |
   | ------------- | ----------------------------------------- |
   | 设定KeyId     | 阿里云 AccessKey ID                       |
   | 设定KeySecret | 阿里云 AccessKey Secret                   |
   | 设定Bucket    | 创建的 Bucket 名称(如下图一的picgo-quill) |
   | 设定存储区域  | 如 `oss-cn-hangzhou`(下图二的)            |
   | 指定存储路径  | Bucket创建的目录（如 `md-images/`）       |

   ![PicGo 配置示例](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250303195748605.png)





![image-20250303201601116](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250303201601116.png)



设为默认图床 → 保存配置

## 四、Typora 配置

1. 打开 Typora → **偏好设置** → **图像**

2. 配置如下参数：

   | 参数       | 值                      |
   | ---------- | ----------------------- |
   | 插入图片时 | 上传图片                |
   | 上传服务   | PicGo（app）            |
   | PicGo 路径 | 选择本地 PicGo 安装路径 |

   ![Typora 配置示例](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/image-20250303200028176.png)

3. 点击 **验证图片上传**，确认成功后保存

## 五、验证与使用

1. **上传测试**：
   - 在 Typora 中粘贴或拖入图片 → 自动上传至 OSS
   - 图片 URL 格式：`https://{Bucket}.{Region}.aliyuncs.com/{Path}/image.png`
2. **查看文件**：
    登录阿里云 OSS → 进入 Bucket 目录 → 确认图片已存在

## 六、注意事项

1. **安全警告**：
   - AccessKey 需严格保密，避免泄露
   - 生产环境建议开启 **子账号 AccessKey** 并限制 OSS 权限
2. **存储费用**：
   - OSS 按存储量和请求次数计费，注意监控用量
3. **路径规范**：
   - 建议使用日期分类路径（如 `md-images/2024/03/`）
4. **如果出现上传失败，请查看两者端口是否一致**

**完成以上配置后，即可实现 Markdown 图片自动上传至阿里云 OSS！**

# RuoYi 专项债管理系统 API接口文档

## 项目概述

本项目基于 **RuoYi v3.8.8** 框架开发，是一个专项债管理系统，提供完整的项目管理、建安管理、模板管理等功能模块。

- **后端技术栈**: Spring Boot 2.5.15, Spring Security, MyBatis, Redis, JWT
- **服务器端口**: 8080
- **API基础路径**: `http://localhost:8080`

## 通用说明

### 1. 认证方式

系统使用 **JWT (JSON Web Token)** 进行身份认证。

- **Token获取**: 调用登录接口 `/login` 获取token
- **Token使用**: 在请求头中添加 `Authorization: Bearer {token}`
- **Token有效期**: 120分钟

### 2. 请求格式

- **Content-Type**: `application/json`
- **字符编码**: UTF-8
- **日期格式**: `yyyy-MM-dd HH:mm:ss`

### 3. 响应格式

所有接口统一返回 `AjaxResult` 格式：

```json
{
  "code": 200,          // 状态码：200成功，500失败
  "msg": "操作成功",     // 提示信息
  "data": {}            // 响应数据
}
```

### 4. 分页参数

列表查询接口支持分页，通过请求参数传递：

- `pageNum`: 页码（默认1）
- `pageSize`: 每页数量（默认10）

分页响应格式 `TableDataInfo`：

```json
{
  "code": 200,
  "msg": "查询成功",
  "rows": [],           // 数据列表
  "total": 100          // 总记录数
}
```

### 5. 权限控制

接口使用 `@PreAuthorize` 注解进行权限控制，格式：`@ss.hasPermi('system:user:list')`

---

## 一、认证授权模块

### 1.1 用户登录

- **接口地址**: `POST /login`
- **接口描述**: 用户登录，获取token
- **请求参数**:

```json
{
  "username": "admin",        // 用户名
  "password": "admin123",     // 密码
  "code": "1234",             // 验证码
  "uuid": "xxxx-xxxx",        // 验证码UUID
  "googleCode": "123456"      // Google验证码（可选）
}
```

- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "token": "eyJhbGciOiJIUzUxMiJ9..."
}
```

---

### 1.2 获取用户信息

- **接口地址**: `GET /getInfo`
- **接口描述**: 获取当前登录用户的详细信息、角色和权限
- **请求参数**: 无
- **权限标识**: 需要认证
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "user": {
    "userId": 1,
    "userName": "admin",
    "nickName": "管理员",
    "email": "admin@example.com",
    "phonenumber": "13800138000",
    "sex": "0",
    "avatar": "",
    "deptId": 100,
    "postIds": [1],
    "roleIds": [1]
  },
  "roles": ["admin"],
  "permissions": ["*:*:*"]
}
```

---

### 1.3 获取路由信息

- **接口地址**: `GET /getRouters`
- **接口描述**: 获取当前用户的动态路由菜单
- **请求参数**: 无
- **权限标识**: 需要认证
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "data": [
    {
      "name": "System",
      "path": "/system",
      "component": "Layout",
      "meta": {
        "title": "系统管理",
        "icon": "system"
      },
      "children": [...]
    }
  ]
}
```

---

### 1.4 获取验证码

- **接口地址**: `GET /captchaImage`
- **接口描述**: 生成图形验证码
- **请求参数**: 无
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "captchaEnabled": true,
  "uuid": "xxxx-xxxx-xxxx",
  "img": "data:image/jpeg;base64,..."
}
```

---

## 二、系统管理模块

### 2.1 用户管理

#### 2.1.1 查询用户列表

- **接口地址**: `GET /system/user/list`
- **接口描述**: 分页查询用户列表
- **请求参数**:
  - `userName`: 用户名（可选）
  - `phonenumber`: 手机号码（可选）
  - `status`: 用户状态（可选）
  - `deptId`: 部门ID（可选）
  - `pageNum`: 页码（可选）
  - `pageSize`: 每页数量（可选）
- **权限标识**: `system:user:list`
- **响应数据**: `TableDataInfo`

---

#### 2.1.2 获取所有用户列表

- **接口地址**: `GET /system/user/allList`
- **接口描述**: 查询所有用户列表（不分页）
- **请求参数**: 同 2.1.1
- **权限标识**: 无
- **响应数据**: `AjaxResult` 包含用户列表

---

#### 2.1.3 获取用户详细信息

- **接口地址**: `GET /system/user/{userId}` 或 `GET /system/user/`
- **接口描述**: 根据用户ID获取详细信息，包含角色和岗位信息
- **请求参数**:
  - `userId`: 用户ID（路径参数，可选）
- **权限标识**: `system:user:query`
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "roles": [...],     // 可选角色列表
  "posts": [...],     // 岗位列表
  "data": {...},      // 用户详细信息
  "postIds": [1,2],   // 用户已分配岗位ID
  "roleIds": [1]      // 用户已分配角色ID
}
```

---

#### 2.1.4 新增用户

- **接口地址**: `POST /system/user`
- **接口描述**: 新增系统用户
- **请求参数**:

```json
{
  "userName": "test",
  "nickName": "测试用户",
  "password": "123456",
  "email": "test@example.com",
  "phonenumber": "13800138001",
  "sex": "0",
  "status": "0",
  "deptId": 100,
  "postIds": [1],
  "roleIds": [2]
}
```

- **权限标识**: `system:user:add`
- **响应数据**: `AjaxResult`

---

#### 2.1.5 修改用户

- **接口地址**: `PUT /system/user`
- **接口描述**: 修改用户信息
- **请求参数**: 同新增用户，需包含 `userId`
- **权限标识**: `system:user:edit`
- **响应数据**: `AjaxResult`

---

#### 2.1.6 删除用户

- **接口地址**: `DELETE /system/user/{userIds}`
- **接口描述**: 批量删除用户
- **请求参数**:
  - `userIds`: 用户ID数组（路径参数，逗号分隔）
- **权限标识**: `system:user:remove`
- **响应数据**: `AjaxResult`

---

#### 2.1.7 重置密码

- **接口地址**: `PUT /system/user/resetPwd`
- **接口描述**: 重置用户密码
- **请求参数**:

```json
{
  "userId": 1,
  "password": "新密码"
}
```

- **权限标识**: `system:user:resetPwd`
- **响应数据**: `AjaxResult`

---

#### 2.1.8 修改用户状态

- **接口地址**: `PUT /system/user/changeStatus`
- **接口描述**: 启用/停用用户
- **请求参数**:

```json
{
  "userId": 1,
  "status": "0"  // 0正常，1停用
}
```

- **权限标识**: `system:user:edit`
- **响应数据**: `AjaxResult`

---

#### 2.1.9 用户授权角色

- **接口地址**: `PUT /system/user/authRole`
- **接口描述**: 为用户分配角色
- **请求参数**:
  - `userId`: 用户ID
  - `roleIds`: 角色ID数组
- **权限标识**: `system:user:edit`
- **响应数据**: `AjaxResult`

---

#### 2.1.10 导出用户数据

- **接口地址**: `POST /system/user/export`
- **接口描述**: 导出用户数据为Excel
- **请求参数**: 查询条件（同查询列表）
- **权限标识**: `system:user:export`
- **响应数据**: Excel文件流

---

#### 2.1.11 导入用户数据

- **接口地址**: `POST /system/user/importData`
- **接口描述**: 从Excel导入用户数据
- **请求参数**:
  - `file`: Excel文件（MultipartFile）
  - `updateSupport`: 是否更新已存在用户（boolean）
- **权限标识**: `system:user:import`
- **响应数据**: `AjaxResult`

---

#### 2.1.12 下载导入模板

- **接口地址**: `POST /system/user/importTemplate`
- **接口描述**: 下载用户导入Excel模板
- **请求参数**: 无
- **权限标识**: 无
- **响应数据**: Excel文件流

---

#### 2.1.13 获取用户Google密钥

- **接口地址**: `GET /system/user/getSysUserGGKey/{userId}`
- **接口描述**: 获取用户的Google Authenticator密钥和二维码
- **请求参数**:
  - `userId`: 用户ID（路径参数）
- **权限标识**: 无
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "codeUrl": "https://...",  // 二维码URL
  "key": "XXXXXXXX"          // 密钥
}
```

---

#### 2.1.14 获取部门树

- **接口地址**: `GET /system/user/deptTree`
- **接口描述**: 获取部门树结构（用于用户管理）
- **请求参数**: 部门查询条件（可选）
- **权限标识**: `system:user:list`
- **响应数据**: `AjaxResult` 包含部门树

---

### 2.2 角色管理

#### 2.2.1 查询角色列表

- **接口地址**: `GET /system/role/list`
- **接口描述**: 分页查询角色列表
- **请求参数**:
  - `roleName`: 角色名称（可选）
  - `roleKey`: 角色权限字符（可选）
  - `status`: 角色状态（可选）
  - `pageNum`, `pageSize`: 分页参数
- **权限标识**: `system:role:list`
- **响应数据**: `TableDataInfo`

---

#### 2.2.2 获取角色详细信息

- **接口地址**: `GET /system/role/{roleId}`
- **接口描述**: 根据角色ID获取详细信息
- **请求参数**:
  - `roleId`: 角色ID（路径参数）
- **权限标识**: `system:role:query`
- **响应数据**: `AjaxResult` 包含角色详情

---

#### 2.2.3 新增角色

- **接口地址**: `POST /system/role`
- **接口描述**: 新增角色
- **请求参数**:

```json
{
  "roleName": "测试角色",
  "roleKey": "test",
  "roleSort": 3,
  "status": "0",
  "menuIds": [1,2,3],
  "deptIds": [100,101],
  "remark": "备注信息"
}
```

- **权限标识**: `system:role:add`
- **响应数据**: `AjaxResult`

---

#### 2.2.4 修改角色

- **接口地址**: `PUT /system/role`
- **接口描述**: 修改角色信息
- **请求参数**: 同新增角色，需包含 `roleId`
- **权限标识**: `system:role:edit`
- **响应数据**: `AjaxResult`

---

#### 2.2.5 删除角色

- **接口地址**: `DELETE /system/role/{roleIds}`
- **接口描述**: 批量删除角色
- **请求参数**:
  - `roleIds`: 角色ID数组（路径参数，逗号分隔）
- **权限标识**: `system:role:remove`
- **响应数据**: `AjaxResult`

---

#### 2.2.6 修改角色状态

- **接口地址**: `PUT /system/role/changeStatus`
- **接口描述**: 启用/停用角色
- **请求参数**:

```json
{
  "roleId": 1,
  "status": "0"
}
```

- **权限标识**: `system:role:edit`
- **响应数据**: `AjaxResult`

---

#### 2.2.7 修改数据权限

- **接口地址**: `PUT /system/role/dataScope`
- **接口描述**: 修改角色的数据权限范围
- **请求参数**:

```json
{
  "roleId": 1,
  "dataScope": "2",  // 1全部 2自定义 3本部门 4本部门及以下 5仅本人
  "deptIds": [100,101]
}
```

- **权限标识**: `system:role:edit`
- **响应数据**: `AjaxResult`

---

#### 2.2.8 获取角色选择框列表

- **接口地址**: `GET /system/role/optionselect`
- **接口描述**: 获取所有角色列表（用于下拉选择）
- **请求参数**: 无
- **权限标识**: `system:role:query`
- **响应数据**: `AjaxResult` 包含角色列表

---

#### 2.2.9 查询已分配用户角色列表

- **接口地址**: `GET /system/role/authUser/allocatedList`
- **接口描述**: 查询已分配某角色的用户列表
- **请求参数**:
  - `roleId`: 角色ID
  - 其他用户查询条件
- **权限标识**: `system:role:list`
- **响应数据**: `TableDataInfo`

---

#### 2.2.10 查询未分配用户角色列表

- **接口地址**: `GET /system/role/authUser/unallocatedList`
- **接口描述**: 查询未分配某角色的用户列表
- **请求参数**: 同 2.2.9
- **权限标识**: `system:role:list`
- **响应数据**: `TableDataInfo`

---

#### 2.2.11 批量选择用户授权

- **接口地址**: `PUT /system/role/authUser/selectAll`
- **接口描述**: 批量为用户分配角色
- **请求参数**:
  - `roleId`: 角色ID
  - `userIds`: 用户ID数组
- **权限标识**: `system:role:edit`
- **响应数据**: `AjaxResult`

---

#### 2.2.12 取消用户授权

- **接口地址**: `PUT /system/role/authUser/cancel`
- **接口描述**: 取消单个用户的角色授权
- **请求参数**:

```json
{
  "roleId": 1,
  "userId": 2
}
```

- **权限标识**: `system:role:edit`
- **响应数据**: `AjaxResult`

---

#### 2.2.13 批量取消用户授权

- **接口地址**: `PUT /system/role/authUser/cancelAll`
- **接口描述**: 批量取消用户的角色授权
- **请求参数**:
  - `roleId`: 角色ID
  - `userIds`: 用户ID数组
- **权限标识**: `system:role:edit`
- **响应数据**: `AjaxResult`

---

#### 2.2.14 获取角色部门树

- **接口地址**: `GET /system/role/deptTree/{roleId}`
- **接口描述**: 获取角色对应的部门树
- **请求参数**:
  - `roleId`: 角色ID（路径参数）
- **权限标识**: `system:role:query`
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "checkedKeys": [100,101],  // 已选中的部门ID
  "depts": [...]             // 部门树结构
}
```

---

#### 2.2.15 导出角色数据

- **接口地址**: `POST /system/role/export`
- **接口描述**: 导出角色数据为Excel
- **请求参数**: 查询条件
- **权限标识**: `system:role:export`
- **响应数据**: Excel文件流

---

### 2.3 菜单管理

#### 2.3.1 查询菜单列表

- **接口地址**: `GET /system/menu/list`
- **接口描述**: 查询菜单列表（树形结构）
- **请求参数**:
  - `menuName`: 菜单名称（可选）
  - `status`: 菜单状态（可选）
- **权限标识**: `system:menu:list`
- **响应数据**: `AjaxResult` 包含菜单树

---

#### 2.3.2 获取菜单详细信息

- **接口地址**: `GET /system/menu/{menuId}`
- **接口描述**: 根据菜单ID获取详细信息
- **请求参数**:
  - `menuId`: 菜单ID（路径参数）
- **权限标识**: `system:menu:query`
- **响应数据**: `AjaxResult` 包含菜单详情

---

#### 2.3.3 获取菜单下拉树

- **接口地址**: `GET /system/menu/treeselect`
- **接口描述**: 获取菜单下拉树（用于选择父菜单）
- **请求参数**: 菜单查询条件（可选）
- **权限标识**: 无
- **响应数据**: `AjaxResult` 包含树形选择器数据

---

#### 2.3.4 加载角色菜单列表树

- **接口地址**: `GET /system/menu/roleMenuTreeselect/{roleId}`
- **接口描述**: 获取角色对应的菜单树
- **请求参数**:
  - `roleId`: 角色ID（路径参数）
- **权限标识**: 无
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "checkedKeys": [1,2,3],  // 已选中的菜单ID
  "menus": [...]           // 菜单树结构
}
```

---

#### 2.3.5 新增菜单

- **接口地址**: `POST /system/menu`
- **接口描述**: 新增菜单
- **请求参数**:

```json
{
  "menuName": "测试菜单",
  "parentId": 0,
  "orderNum": 1,
  "path": "/test",
  "component": "test/index",
  "isFrame": "1",
  "isCache": "0",
  "menuType": "C",  // M目录 C菜单 F按钮
  "visible": "0",
  "status": "0",
  "perms": "system:test:list",
  "icon": "test"
}
```

- **权限标识**: `system:menu:add`
- **响应数据**: `AjaxResult`

---

#### 2.3.6 修改菜单

- **接口地址**: `PUT /system/menu`
- **接口描述**: 修改菜单信息
- **请求参数**: 同新增菜单，需包含 `menuId`
- **权限标识**: `system:menu:edit`
- **响应数据**: `AjaxResult`

---

#### 2.3.7 删除菜单

- **接口地址**: `DELETE /system/menu/{menuId}`
- **接口描述**: 删除菜单
- **请求参数**:
  - `menuId`: 菜单ID（路径参数）
- **权限标识**: `system:menu:remove`
- **响应数据**: `AjaxResult`

---

### 2.4 部门管理

**Controller路径**: `/system/dept`

提供部门的增删改查、部门树查询等功能。标准CRUD接口，参考用户管理模式。

---

### 2.5 岗位管理

**Controller路径**: `/system/post`

提供岗位的增删改查、岗位选择列表等功能。标准CRUD接口。

---

### 2.6 字典管理

#### 2.6.1 字典类型管理

**Controller路径**: `/system/dict/type`

提供字典类型的增删改查、导出、刷新缓存等功能。

主要接口：
- `GET /system/dict/type/list` - 查询字典类型列表
- `POST /system/dict/type` - 新增字典类型
- `PUT /system/dict/type` - 修改字典类型
- `DELETE /system/dict/type/{dictIds}` - 删除字典类型
- `GET /system/dict/type/refreshCache` - 刷新字典缓存
- `GET /system/dict/type/optionselect` - 获取字典选择框列表

---

#### 2.6.2 字典数据管理

**Controller路径**: `/system/dict/data`

提供字典数据的增删改查等功能。

主要接口：
- `GET /system/dict/data/list` - 查询字典数据列表
- `GET /system/dict/data/type/{dictType}` - 根据字典类型查询数据
- `POST /system/dict/data` - 新增字典数据
- `PUT /system/dict/data` - 修改字典数据
- `DELETE /system/dict/data/{dictCodes}` - 删除字典数据

---

### 2.7 参数配置

**Controller路径**: `/system/config`

提供系统参数配置的管理功能。

主要接口：
- `GET /system/config/list` - 查询参数列表
- `GET /system/config/{configId}` - 获取参数详情
- `GET /system/config/configKey/{configKey}` - 根据参数键名查询值
- `POST /system/config` - 新增参数
- `PUT /system/config` - 修改参数
- `DELETE /system/config/{configIds}` - 删除参数
- `GET /system/config/refreshCache` - 刷新参数缓存

---

### 2.8 通知公告

**Controller路径**: `/system/notice`

提供系统通知公告的管理功能。标准CRUD接口。

---

### 2.9 个人信息管理

**Controller路径**: `/system/user/profile`

提供当前用户个人信息、密码修改、头像上传等功能。

---

### 2.10 地区管理

**Controller路径**: `/system/area`

提供地区信息的管理功能。

---

### 2.11 投资管理

**Controller路径**: `/system/investment`

提供投资信息的管理功能。

---

### 2.12 项目建设管理

**Controller路径**: `/system/projectConstruction`

提供项目建设信息的管理功能。

---

### 2.13 文件模板管理

**Controller路径**: `/system/fileTemplate`

提供文件模板的管理功能。

---

## 三、项目管理模块

### 3.1 项目信息管理

#### 3.1.1 查询项目列表

- **接口地址**: `GET /project/info/list`
- **接口描述**: 分页查询项目信息列表
- **请求参数**:
  - 项目查询条件（项目名称、状态等）
  - `pageNum`, `pageSize`: 分页参数
- **权限标识**: `project:info:list`
- **响应数据**: `TableDataInfo`

---

#### 3.1.2 获取项目详细信息

- **接口地址**: `GET /project/info/{id}`
- **接口描述**: 根据项目ID获取详细信息
- **请求参数**:
  - `id`: 项目ID（路径参数）
- **权限标识**: `project:info:query`
- **响应数据**: `AjaxResult` 包含项目详情

---

#### 3.1.3 新增项目

- **接口地址**: `POST /project/info`
- **接口描述**: 新增项目信息
- **请求参数**: `ProjectInfoDto` 对象（JSON格式）
- **权限标识**: `project:info:add`
- **响应数据**: `AjaxResult`

---

#### 3.1.4 修改项目

- **接口地址**: `PUT /project/info`
- **接口描述**: 修改项目信息
- **请求参数**: `ProjectInfoDto` 对象（JSON格式）
- **权限标识**: `project:info:edit`
- **响应数据**: `AjaxResult`

---

#### 3.1.5 删除项目

- **接口地址**: `DELETE /project/info/{ids}`
- **接口描述**: 批量删除项目
- **请求参数**:
  - `ids`: 项目ID数组（路径参数，逗号分隔）
- **权限标识**: `project:info:remove`
- **响应数据**: `AjaxResult`

---

#### 3.1.6 批量修改项目状态

- **接口地址**: `POST /project/info/updatebathStatus`
- **接口描述**: 批量修改项目状态
- **请求参数**: `ProjectInfoDto` 对象
- **权限标识**: `project:info:edit`
- **响应数据**: `AjaxResult`

---

#### 3.1.7 项目审核

- **接口地址**: `POST /project/info/audit`
- **接口描述**: 审核项目信息
- **请求参数**: `AuditProjectInfoDto` 对象
- **权限标识**: `project:info:edit`
- **响应数据**: `AjaxResult`
- **特殊说明**:
  - 返回 -1: 未检测到测算表
  - 返回 0: 项目状态已更改

---

#### 3.1.8 设置项目编制

- **接口地址**: `POST /project/info/setCompile`
- **接口描述**: 设置项目编制信息
- **请求参数**: `SetCompileDto` 对象
- **权限标识**: `project:info:edit`
- **响应数据**: `AjaxResult`

---

#### 3.1.9 生成项目文档

- **接口地址**: `GET /project/info/createDocment/{id}/{type}`
- **接口描述**: 生成项目相关文档
- **请求参数**:
  - `id`: 项目ID（路径参数）
  - `type`: 文档类型（路径参数）
- **权限标识**: `project:info:query`
- **响应数据**: `AjaxResult`
- **特殊说明**:
  - 返回 800: 未生成投资估算表

---

#### 3.1.10 获取覆盖倍数

- **接口地址**: `GET /project/info/getFgbs/{id}`
- **接口描述**: 获取项目覆盖倍数
- **请求参数**:
  - `id`: 项目ID（路径参数）
- **权限标识**: `project:info:query`
- **响应数据**: `AjaxResult`

---

#### 3.1.11 生成测算表

- **接口地址**: `GET /project/info/createCsb/{ids}`
- **接口描述**: 批量生成项目测算表
- **请求参数**:
  - `ids`: 项目ID数组（路径参数）
- **权限标识**: 无
- **响应数据**: `AjaxResult`

---

#### 3.1.12 查询待办事项

- **接口地址**: `GET /project/info/toDoItems`
- **接口描述**: 查询项目待办事项列表
- **请求参数**: 无
- **权限标识**: `project:info:query`
- **响应数据**: `TableDataInfo`

---

#### 3.1.13 复制项目

- **接口地址**: `PUT /project/info/copy/{id}`
- **接口描述**: 复制项目信息
- **请求参数**:
  - `id`: 项目ID（路径参数）
- **权限标识**: `project:info:adjust`
- **响应数据**: `AjaxResult`

---

#### 3.1.14 导出项目数据

- **接口地址**: `POST /project/info/export`
- **接口描述**: 导出项目数据（功能未完全实现）
- **请求参数**: 查询条件
- **权限标识**: `project:info:export`

---

### 3.2 项目详细信息管理

**Controller路径**: `/project/detail`

提供项目详细信息的管理功能。标准CRUD接口。

---

### 3.3 项目文件管理

**Controller路径**: `/project/file`

提供项目相关文件上传、下载、删除等功能。

---

### 3.4 项目收入管理

**Controller路径**: `/project/revenue`

提供项目收入信息的管理功能。

---

### 3.5 收入要素管理

**Controller路径**: `/project/revenueElement`

提供收入要素的配置和管理功能。

---

### 3.6 科研项目管理

**Controller路径**: `/project/keyan`

提供科研项目信息的管理功能。

---

## 四、建安管理模块

### 4.1 建安信息管理

#### 4.1.1 查询建安列表

- **接口地址**: `GET /construction/info/list`
- **接口描述**: 分页查询建安信息列表
- **请求参数**:
  - 查询条件（可选）
  - `pageNum`, `pageSize`: 分页参数
- **权限标识**: `construction:info:list`
- **响应数据**: `TableDataInfo`

---

#### 4.1.2 获取建安详细信息

- **接口地址**: `GET /construction/info/{id}`
- **接口描述**: 根据ID获取建安详细信息
- **请求参数**:
  - `id`: 建安ID（路径参数）
- **权限标识**: `construction:info:query`
- **响应数据**: `AjaxResult`

---

#### 4.1.3 新增建安

- **接口地址**: `POST /construction/info`
- **接口描述**: 新增建安信息
- **请求参数**: `ConstructionInfoDto` 对象
- **权限标识**: `construction:info:add`
- **响应数据**: `AjaxResult`

---

#### 4.1.4 修改建安

- **接口地址**: `PUT /construction/info`
- **接口描述**: 修改建安信息
- **请求参数**: `ConstructionInfo` 对象
- **权限标识**: `construction:info:edit`
- **响应数据**: `AjaxResult`

---

#### 4.1.5 删除建安

- **接口地址**: `DELETE /construction/info/{ids}`
- **接口描述**: 批量删除建安信息
- **请求参数**:
  - `ids`: 建安ID数组（路径参数）
- **权限标识**: `construction:info:remove`
- **响应数据**: `AjaxResult`

---

#### 4.1.6 导出建安数据

- **接口地址**: `POST /construction/info/export`
- **接口描述**: 导出建安数据为Excel
- **请求参数**: 查询条件
- **权限标识**: `construction:info:export`
- **响应数据**: Excel文件流

---

### 4.2 建安要素管理

**Controller路径**: `/construction/element`

提供建安要素的配置和管理功能。标准CRUD接口。

---

## 五、模板管理模块

### 5.1 收入模板管理

#### 5.1.1 查询收入模板列表

- **接口地址**: `GET /template/revenue/list`
- **接口描述**: 分页查询收入模板列表
- **请求参数**:
  - 查询条件（可选）
  - `pageNum`, `pageSize`: 分页参数
- **权限标识**: `template:revenue:list`
- **响应数据**: `TableDataInfo`

---

#### 5.1.2 获取收入模板详细信息

- **接口地址**: `GET /template/revenue/{id}`
- **接口描述**: 根据ID获取收入模板详细信息
- **请求参数**:
  - `id`: 模板ID（路径参数）
- **权限标识**: `template:revenue:query`
- **响应数据**: `AjaxResult`

---

#### 5.1.3 获取收入模板（不含ID）

- **接口地址**: `GET /template/revenue/noId/{id}`
- **接口描述**: 获取收入模板详情（用于复制场景）
- **请求参数**:
  - `id`: 模板ID（路径参数）
- **权限标识**: `template:revenue:query`
- **响应数据**: `AjaxResult`

---

#### 5.1.4 新增收入模板

- **接口地址**: `POST /template/revenue`
- **接口描述**: 新增收入模板
- **请求参数**: `RevenueTemplateDto` 对象
- **权限标识**: `template:revenue:add`
- **响应数据**: `AjaxResult`

---

#### 5.1.5 修改收入模板

- **接口地址**: `PUT /template/revenue`
- **接口描述**: 修改收入模板
- **请求参数**: `RevenueTemplateDto` 对象
- **权限标识**: `template:revenue:edit`
- **响应数据**: `AjaxResult`

---

#### 5.1.6 删除收入模板

- **接口地址**: `DELETE /template/revenue/{ids}`
- **接口描述**: 批量删除收入模板
- **请求参数**:
  - `ids`: 模板ID数组（路径参数）
- **权限标识**: `template:revenue:remove`
- **响应数据**: `AjaxResult`

---

#### 5.1.7 导出收入模板

- **接口地址**: `POST /template/revenue/export`
- **接口描述**: 导出收入模板为Excel
- **请求参数**: 查询条件
- **权限标识**: `template:revenue:export`
- **响应数据**: Excel文件流

---

### 5.2 收入模板要素管理

**Controller路径**: `/template/revenueElement`

提供收入模板要素的管理功能。

---

### 5.3 收入要素模板管理

**Controller路径**: `/template/revenueElementTemplate`

提供收入要素模板的配置功能。

---

### 5.4 建安模板管理

**Controller路径**: `/template/construction`

提供建安模板的管理功能。标准CRUD接口。

---

### 5.5 建安要素模板管理

**Controller路径**: `/template/constructionElement`

提供建安要素模板的管理功能。

---

### 5.6 建安作法管理

**Controller路径**: `/template/constructionZuofa`

提供建安作法（施工方法）的管理功能。

---

### 5.7 支出模板管理

**Controller路径**: `/template/expenditure`

提供支出模板的管理功能。

---

### 5.8 一级分类模板管理

**Controller路径**: `/template/levelOne`

提供一级分类模板的管理功能。

---

## 六、监控管理模块

### 6.1 在线用户监控

**Controller路径**: `/monitor/online`

主要接口：
- `GET /monitor/online/list` - 查询在线用户列表
- `DELETE /monitor/online/{tokenId}` - 强退用户

**权限标识**: `monitor:online:list`, `monitor:online:forceLogout`

---

### 6.2 操作日志

**Controller路径**: `/monitor/operlog`

主要接口：
- `GET /monitor/operlog/list` - 查询操作日志列表
- `DELETE /monitor/operlog/{operIds}` - 删除操作日志
- `DELETE /monitor/operlog/clean` - 清空操作日志
- `POST /monitor/operlog/export` - 导出操作日志

**权限标识**: `monitor:operlog:list`, `monitor:operlog:remove`, `monitor:operlog:export`

---

### 6.3 登录日志

**Controller路径**: `/monitor/logininfor`

主要接口：
- `GET /monitor/logininfor/list` - 查询登录日志列表
- `DELETE /monitor/logininfor/{infoIds}` - 删除登录日志
- `DELETE /monitor/logininfor/clean` - 清空登录日志
- `POST /monitor/logininfor/export` - 导出登录日志
- `POST /monitor/logininfor/unlock/{userName}` - 解锁用户

**权限标识**: `monitor:logininfor:list`, `monitor:logininfor:remove`, `monitor:logininfor:export`, `monitor:logininfor:unlock`

---

### 6.4 服务器监控

#### 6.4.1 获取服务器信息

- **接口地址**: `GET /monitor/server`
- **接口描述**: 获取服务器硬件和软件信息（CPU、内存、磁盘、JVM等）
- **请求参数**: 无
- **权限标识**: `monitor:server:list`
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "data": {
    "cpu": {
      "cpuNum": 8,
      "used": 25.5,
      "sys": 10.2,
      "free": 74.5
    },
    "mem": {
      "total": 16384.0,
      "used": 8192.0,
      "free": 8192.0,
      "usage": 50.0
    },
    "jvm": {...},
    "sys": {...},
    "sysFiles": [...]
  }
}
```

---

### 6.5 缓存监控

**Controller路径**: `/monitor/cache`

主要接口：
- `GET /monitor/cache` - 获取缓存信息
- `GET /monitor/cache/getNames` - 获取缓存名称列表
- `GET /monitor/cache/getKeys/{cacheName}` - 获取缓存键列表
- `GET /monitor/cache/getValue/{cacheName}/{cacheKey}` - 获取缓存值
- `DELETE /monitor/cache/clearCacheName/{cacheName}` - 清空指定缓存
- `DELETE /monitor/cache/clearCacheKey/{cacheName}/{cacheKey}` - 删除指定缓存键
- `DELETE /monitor/cache/clearCacheAll` - 清空所有缓存

**权限标识**: `monitor:cache:list`, `monitor:cache:remove`

---

## 七、定时任务模块

### 7.1 定时任务管理

**Controller路径**: `/monitor/job`

主要接口：
- `GET /monitor/job/list` - 查询定时任务列表
- `GET /monitor/job/{jobId}` - 获取任务详情
- `POST /monitor/job` - 新增定时任务
- `PUT /monitor/job` - 修改定时任务
- `DELETE /monitor/job/{jobIds}` - 删除定时任务
- `PUT /monitor/job/changeStatus` - 修改任务状态
- `PUT /monitor/job/run` - 立即执行任务
- `POST /monitor/job/export` - 导出任务列表

**权限标识**: `monitor:job:list`, `monitor:job:add`, `monitor:job:edit`, `monitor:job:remove`, `monitor:job:changeStatus`, `monitor:job:query`, `monitor:job:export`

---

### 7.2 定时任务日志

**Controller路径**: `/monitor/jobLog`

主要接口：
- `GET /monitor/jobLog/list` - 查询任务日志列表
- `GET /monitor/jobLog/{jobLogId}` - 获取日志详情
- `DELETE /monitor/jobLog/{jobLogIds}` - 删除任务日志
- `DELETE /monitor/jobLog/clean` - 清空任务日志
- `POST /monitor/jobLog/export` - 导出任务日志

**权限标识**: `monitor:job:list`, `monitor:job:query`, `monitor:job:remove`, `monitor:job:export`

---

## 八、代码生成模块

### 8.1 代码生成管理

**Controller路径**: `/tool/gen`

主要接口：
- `GET /tool/gen/list` - 查询代码生成列表
- `GET /tool/gen/db/list` - 查询数据库表列表
- `GET /tool/gen/{talbleId}` - 获取表详细信息
- `POST /tool/gen/importTable` - 导入表结构
- `PUT /tool/gen` - 修改代码生成配置
- `DELETE /tool/gen/{tableIds}` - 删除代码生成配置
- `GET /tool/gen/preview/{tableId}` - 预览生成代码
- `GET /tool/gen/download/{tableName}` - 下载生成代码
- `GET /tool/gen/genCode/{tableName}` - 生成代码（到本地项目）
- `GET /tool/gen/synchDb/{tableName}` - 同步数据库
- `GET /tool/gen/batchGenCode` - 批量生成代码

**权限标识**: `tool:gen:list`, `tool:gen:query`, `tool:gen:import`, `tool:gen:edit`, `tool:gen:remove`, `tool:gen:preview`, `tool:gen:code`

---

## 九、通用工具模块

### 9.1 文件上传下载

#### 9.1.1 通用文件上传（单个）

- **接口地址**: `POST /common/upload`
- **接口描述**: 上传单个文件
- **请求参数**:
  - `file`: 文件（MultipartFile）
- **权限标识**: 无
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "url": "http://localhost:8080/profile/upload/2024/10/27/xxx.jpg",
  "fileName": "/profile/upload/2024/10/27/xxx.jpg",
  "newFileName": "xxx.jpg",
  "originalFilename": "test.jpg"
}
```

---

#### 9.1.2 通用文件上传（多个）

- **接口地址**: `POST /common/uploads`
- **接口描述**: 批量上传文件
- **请求参数**:
  - `files`: 文件列表（List<MultipartFile>）
- **权限标识**: 无
- **响应数据**:

```json
{
  "code": 200,
  "msg": "操作成功",
  "urls": "url1,url2,url3",
  "fileNames": "name1,name2,name3",
  "newFileNames": "newName1,newName2,newName3",
  "originalFilenames": "original1,original2,original3"
}
```

---

#### 9.1.3 通用文件下载

- **接口地址**: `GET /common/download`
- **接口描述**: 下载文件
- **请求参数**:
  - `fileName`: 文件名
  - `delete`: 是否下载后删除（boolean，可选）
- **权限标识**: 无
- **响应数据**: 文件流

---

#### 9.1.4 下载项目文件

- **接口地址**: `GET /common/downloadFile/{id}`
- **接口描述**: 根据文件ID下载项目文件
- **请求参数**:
  - `id`: 文件ID（路径参数）
- **权限标识**: 无
- **响应数据**: 文件流

---

#### 9.1.5 资源文件下载

- **接口地址**: `GET /common/download/resource`
- **接口描述**: 下载本地资源文件
- **请求参数**:
  - `resource`: 资源路径
- **权限标识**: 无
- **响应数据**: 文件流

---

### 9.2 短信验证码

**Controller路径**: `/tool/smsCode`

提供短信验证码发送和验证功能。

---

### 9.3 测试工具

**Controller路径**: `/tool/test`

提供测试相关的工具接口。

---

## 十、用户注册模块

**Controller路径**: `/register`

提供用户注册功能。

主要接口：
- `POST /register` - 用户注册

---

## 十一、首页/看板模块

**Controller路径**: `/system/index`

提供系统首页数据和看板信息。

---

## 附录

### A. 错误码说明

| 错误码 | 说明 |
|-------|------|
| 200 | 操作成功 |
| 500 | 系统内部错误 |
| 401 | 未授权，请重新登录 |
| 403 | 权限不足 |
| 404 | 资源不存在 |
| 800 | 自定义错误码（如：未生成投资估算表） |

### B. 权限标识格式

权限标识采用三段式格式：`模块:功能:操作`

例如：
- `system:user:list` - 系统管理:用户管理:查询列表
- `project:info:add` - 项目管理:项目信息:新增
- `monitor:server:list` - 系统监控:服务器监控:查询

### C. 数据权限说明

系统支持以下数据权限范围：

1. 全部数据权限（dataScope=1）
2. 自定义数据权限（dataScope=2）
3. 本部门数据权限（dataScope=3）
4. 本部门及以下数据权限（dataScope=4）
5. 仅本人数据权限（dataScope=5）

### D. 日志记录说明

系统使用 `@Log` 注解记录操作日志，包含以下业务类型：

- `INSERT` - 新增
- `UPDATE` - 修改
- `DELETE` - 删除
- `GRANT` - 授权
- `EXPORT` - 导出
- `IMPORT` - 导入
- `FORCE` - 强退
- `GENCODE` - 生成代码
- `CLEAN` - 清空数据

### E. 文件上传限制

- 单个文件最大：10MB
- 总大小最大：20MB
- 支持的文件类型：根据系统配置

---

**文档版本**: v1.0
**最后更新**: 2025-10-27
**系统版本**: RuoYi v3.8.8

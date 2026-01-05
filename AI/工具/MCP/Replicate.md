配图不太好找,完全可以让AI帮我们先生成一张图片,这样可以改善产品的美观度

这里我用的图片生成网站是**replicate**,它提供API的调用方式来生成AI图片

官网： https://replicate.com/

github项目： https://github.com/awkoy/replicate-flux-mcp



# claude code

```
claude mcp add replicate npx replicate-flux-mcp --scope user
"replicate": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "replicate-flux-mcp"
      ],
      "env": {
        "REPLICATE_API_TOKEN": "your-api-token-here"
      }
    }
```



```plain
formData = {
  projectConstructionList: [
    {
      name: "土建工程",           // 外层 item.name
      elementDtoList: [          // 外层 item.elementDtoList
        {
          name: "混凝土",         // 内层 item.name（修改前）→ subItem.name（修改后）
          num: 100,              // 内层 item.num → subItem.num
          price: 500,            // 内层 item.price → subItem.price
          unit: "立方米",         // 内层 item.unit → subItem.unit
          remake: "C30混凝土",   // 内层 item.remake → subItem.remake ⚠️问题核心
          status: 1              // 内层 item.status → subItem.status
        },
        {
          name: "钢筋",
          num: 50,
          price: 4000,
          unit: "吨",
          remake: "HRB400钢筋",  // ⚠️ 这就是您说的备注框
          status: 1
        }
      ]
    },
    {
      name: "安装工程",
      elementDtoList: [
        {
          name: "电线",
          num: 1000,
          price: 5,
          unit: "米",
          remake: "BV2.5平方",
          status: 1
        }
      ]
    }
  ]
}
```
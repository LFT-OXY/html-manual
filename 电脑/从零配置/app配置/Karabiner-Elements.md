
[教程](https://www.bilibili.com/video/BV1K5411A7yY/?vd_source=8091dc504148a4adc6d9191fa62d7571)

```
~/.config/karabiner/karabiner.json
```


![image.png|1000](https://picgo-q1uill.oss-cn-chengdu.aliyuncs.com/img-for-typora/20250419160828914.png)



```
{
    "profiles": [
        {
            "complex_modifications": {
                "parameters": { "basic.to_if_held_down_threshold_milliseconds": 300 },
                "rules": [
                    {
                        "description": "自定义：轻按右移键到 Cmd+Shift+2",
                        "enabled": false,
                        "manipulators": [
                            {
                                "from": {
                                    "key_code": "right_shift",
                                    "modifiers": { "optional": ["any"] }
                                },
                                "to": [{ "key_code": "right_shift" }],
                                "to_if_alone": [
                                    {
                                        "key_code": "2",
                                        "modifiers": ["left_command", "left_shift"]
                                    }
                                ],
                                "type": "basic"
                            }
                        ]
                    },
                    {
                        "description": "Mac OSX: 左键 cmd + 双击 q → 关闭应用程序 (Double-press Cmd+Q to Quit)",
                        "manipulators": [
                            {
                                "conditions": [
                                    {
                                        "name": "command_q_pressed_once",
                                        "type": "variable_if",
                                        "value": 1
                                    }
                                ],
                                "from": {
                                    "key_code": "q",
                                    "modifiers": { "mandatory": ["command"] }
                                },
                                "to": [
                                    {
                                        "key_code": "q",
                                        "modifiers": ["left_command"]
                                    }
                                ],
                                "type": "basic"
                            },
                            {
                                "from": {
                                    "key_code": "q",
                                    "modifiers": { "mandatory": ["command"] }
                                },
                                "to": [
                                    {
                                        "set_variable": {
                                            "name": "command_q_pressed_once",
                                            "value": 1
                                        }
                                    }
                                ],
                                "to_delayed_action": {
                                    "to_if_canceled": [
                                        {
                                            "set_variable": {
                                                "name": "command_q_pressed_once",
                                                "value": 0
                                            }
                                        }
                                    ],
                                    "to_if_invoked": [
                                        {
                                            "set_variable": {
                                                "name": "command_q_pressed_once",
                                                "value": 0
                                            }
                                        }
                                    ]
                                },
                                "type": "basic"
                            }
                        ]
                    },
                    {
                        "description": "自定义：长按左Shift键为left_control（轻点与普通 Shift 键相同）",
                        "enabled": false,
                        "manipulators": [
                            {
                                "from": {
                                    "key_code": "left_shift",
                                    "modifiers": { "optional": ["any"] }
                                },
                                "to": [{ "key_code": "left_shift" }],
                                "to_if_held_down": [{ "key_code": "left_control" }],
                                "type": "basic"
                            }
                        ]
                    }
                ]
            },
            "devices": [
                {
                    "identifiers": {
                        "is_keyboard": true,
                        "is_pointing_device": true,
                        "product_id": 13873,
                        "vendor_id": 21320
                    },
                    "ignore": false,
                    "simple_modifications": [
                        {
                            "from": { "key_code": "left_command" },
                            "to": [{ "key_code": "left_option" }]
                        },
                        {
                            "from": { "key_code": "left_control" },
                            "to": [{ "key_code": "left_command" }]
                        },
                        {
                            "from": { "key_code": "right_option" },
                            "to": [{ "key_code": "right_control" }]
                        }
                    ]
                },
                {
                    "identifiers": {
                        "is_keyboard": true,
                        "product_id": 13873,
                        "vendor_id": 21320
                    },
                    "ignore": true
                },
                {
                    "identifiers": {
                        "is_keyboard": true,
                        "product_id": 53344,
                        "vendor_id": 21675
                    },
                    "simple_modifications": [
                        {
                            "from": { "key_code": "left_command" },
                            "to": [{ "key_code": "left_option" }]
                        },
                        {
                            "from": { "key_code": "left_control" },
                            "to": [{ "key_code": "left_command" }]
                        },
                        {
                            "from": { "key_code": "left_option" },
                            "to": [{ "key_code": "left_control" }]
                        },
                        {
                            "from": { "key_code": "right_option" },
                            "to": [{ "key_code": "right_control" }]
                        }
                    ]
                },
                {
                    "identifiers": {
                        "is_keyboard": true,
                        "product_id": 22345,
                        "vendor_id": 21675
                    },
                    "simple_modifications": [
                        {
                            "from": { "key_code": "left_command" },
                            "to": [{ "key_code": "left_option" }]
                        },
                        {
                            "from": { "key_code": "left_control" },
                            "to": [{ "key_code": "left_command" }]
                        },
                        {
                            "from": { "key_code": "left_option" },
                            "to": [{ "key_code": "left_control" }]
                        },
                        {
                            "from": { "key_code": "right_option" },
                            "to": [{ "key_code": "right_control" }]
                        }
                    ]
                },
                {
                    "identifiers": {
                        "is_keyboard": true,
                        "product_id": 38,
                        "vendor_id": 41219
                    },
                    "simple_modifications": [
                        {
                            "from": { "key_code": "left_command" },
                            "to": [{ "key_code": "left_option" }]
                        },
                        {
                            "from": { "key_code": "left_control" },
                            "to": [{ "key_code": "left_command" }]
                        },
                        {
                            "from": { "key_code": "left_option" },
                            "to": [{ "key_code": "left_control" }]
                        },
                        {
                            "from": { "key_code": "right_option" },
                            "to": [{ "key_code": "right_control" }]
                        }
                    ]
                }
            ],
            "name": "Default profile",
            "selected": true,
            "virtual_hid_keyboard": { "keyboard_type_v2": "ansi" }
        }
    ]
}
```
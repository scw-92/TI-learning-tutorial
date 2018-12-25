# LCD
## 01 配置过程
* 01 启动宏定义
* 02 配置时钟
* 03 LCD引脚配置

## 02 启动宏定义
```sh
    NFIG_LCD_UBOOT
```

## 03 配置显示子系统时钟
```sh
  arch/arm/cpu/armv7/am33xx/clock_am33xx.c
```

```sh
# 01 预定义显示子系统的时钟寄存器
const struct dpll_regs dpll_dis_regs = {
		.cm_clkmode_dpll = CM_WKUP + 0x98,
		.cm_idlest_dpll  = CM_WKUP + 0x48,
		.cm_clksel_dpll  = CM_WKUP + 0x54,
		.cm_div_m2_dpll  = CM_WKUP + 0xA4,
	};
	```

  ```sh
  # 02 预定于显示子系统的时钟频率
	const struct dpll_params dpll_dis = {100, OSC-1, 1, -1, -1, -1, -1};

	const struct dpll_params *get_dpll_dis_params(void)
	{
		return &dpll_dis;
	}
	```

  ```sh
    # 03 设置显示子系统的时钟频率
    params = get_dpll_dis_params();
    do_setup_dpll(&dpll_dis_regs, params);
  ```

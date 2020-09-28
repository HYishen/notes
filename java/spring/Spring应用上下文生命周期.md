### Spring应用上下文启动准备阶段
- AbstractApplicationContext#prepareRefresh()方法
  - 启动时间 - startupDate
  - 状态表示 - closed(false)、active(true)
  - 初始化PropertySources - initPropertySources()
  - 校验Environment中必须属性
  - 初始化事件监听器集合
  - 初始化早期Spring事件集合


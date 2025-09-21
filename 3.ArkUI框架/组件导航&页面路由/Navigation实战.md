## 一、Stack模式

### （一）、新建一个页面要新增main_pages.json



新增一个@Entry页面，就需要在main_pages.json中新增配置；

main_pages.json的作用？

和loadContent之间的对应关系？





### （二）、Navigation下层放置一个NavDestination，完全没法跳转





测试1：

Navigation下进行pushPath，可以正常跳转



Navigation-NavDestination下进行pushPath，完全没法跳转





### （三）、NavPathStack对象要使用@Provide @Consume进行传递

```
NavPathStack
```

NavPathStack 对象，和Navigation绑定之后，如果每个NavDestination页面要使用，则需要使用@Provide、@Consume获取，而不是重新创建，重现创建的话就会出现无法跳转的问题，因为它们不在同一个栈上，页面跳转是无法体现的。





### （四）、NavDestination跳转页面默认横屏

在目标 NavDestination 页面的生命周期回调中（如 `aboutToAppear`），通过 `window.getLastWindow()` 获取窗口实例，调用 `setPreferredOrientation` 接口强制设置横屏：



```typescript
  async aboutToAppear() {
    try {
      // 获取当前窗口实例
      const mainWindow = await window.getLastWindow();
      // 设置为横屏（强制横屏，不受系统方向影响）
      await mainWindow.setPreferredOrientation(window.Orientation.LANDSCAPE);
    } catch (err) {
      console.error('设置横屏失败:', err);
    }
```



注意！

**在当前横屏页面退出后，要及时将window的优先朝向改为竖屏，避免当前window下的其他page存在布局问题！**





## 二、Split分屏模式
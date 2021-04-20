全限定名：org.springframework.context.support.AbstractApplicationContext  
```text
ApplicationContext接口
        ｜--ConfigurableApplicationContext接口
                    ｜--AbstractApplicationContext模抽象版类
```

抽象模版类，并非接口，不是扩展点，只是其中的模版方法refresh()会在项目启动过程中的这个时机被调用  
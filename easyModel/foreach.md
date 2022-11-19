### 遍历器
既可以不暴露维护的集合，又能供外界访问，可以使用类似下面这样的遍历器。这样通过回调函数，让调用者定义访问逻辑，而Foreach则提供维护的集合元素。
```go
func (l *Example) Foreach(f func(key string,value string)bool) {
    l.lock.RLock()
    defer l.lock.RUnlock()

    for k,v := range l.data{
        if !f(k,v){
            break
        }
    }
}
```
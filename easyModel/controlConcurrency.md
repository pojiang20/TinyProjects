### 控制并发模型

通过`ConcurrencyControl`控制并发，任务开始执行时消费一个空间，任务结束返还一个空间。
```go
func NewRunner() (*CountRunner, error) {
	return &CountRunner{
		InputQ:             make(chan *Task, 10),
		CompleteQ:          make(chan *Task, 10),
		ConcurrencyControl: make(chan struct{}, 10),
	}, nil
}
```
```go
func (th *Runner) Run() {
	for {
		th.ConcurrencyControl <- struct{}{}
		task := <-th.InputQ
		go func() {
			th.CompleteQ <- th.run(task)
			<-th.ConcurrencyControl
		}()
	}
}
```
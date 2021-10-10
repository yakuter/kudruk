# kudruk
Channels are widely used as queues. `kudruk` (means queue in Turkish) helps you to easily create queue with channel and manage the data in the queue. You don't have to be afraid of panic situations like channel is already closed etc.

## Usage

```go
func main() {
	callbackFn := func(data interface{}) error {
		fmt.Printf("Processed data: %v\n", data)
		return nil
	}

	opts := &kudruk.Options{
		CallbackFn: callbackFn,
		Limit:      10,
	}

	ctx, cancel := context.WithCancel(context.Background())
	defer cancel()

	queue := kudruk.New(ctx, opts)

	go queue.Listen()

	for i := 0; i <= 20; i++ {
		queue.Add(fmt.Sprintf("job-%d", i))
	}
}
```
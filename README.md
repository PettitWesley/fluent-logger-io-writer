# fluent-logger-golang-example

Go Code that wraps the fluent-logger-golang in a struct that implements [io.Writer](https://golang.org/pkg/io/).

This means it can be used as the underlying io stream for many loggers. See [main.go](main.go) for full example usage.

This project is part of a blog post/tutorial on Fluentd & Fluent Bit: **TODO add link.**

Simple example with the popular `sirupsen/logrus` logger:

```
import (
	"fmt"

	"github.com/PettitWesley/fluent-logger-io-writer/logger"

	"github.com/fluent/fluent-logger-golang/fluent"
	"github.com/sirupsen/logrus"
)

func main() {
	// configure logrus to output as JSON
	logrus.SetFormatter(&logrus.JSONFormatter{})
	logrus.SetLevel(logrus.DebugLevel)

	// create a FluentWriter instance
	fluentLogger, err := logger.NewFluentWriter(fluent.Config{}, "app", []string{"level"})
	if err != nil {
		fmt.Println(err)
		os.Exit(1)
	}

	// Set logrus to use it
	logrus.SetOutput(fluentLogger)

	// Now use logrus as normal!
	logrus.WithFields(logrus.Fields{
		"animal": "walrus",
	}).Info("A walrus appears")
}
```
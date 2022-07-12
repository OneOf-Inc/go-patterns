## Futures & Promises Pattern
Futures pattern is analogous to async/await in JS. In futures pattern, we launch goroutines to carry on tasks beforehand and we use the result later somewhere in the application. This pattern is useful to make code non-blocking in cases where we have some dependency on some external infrastructure e.g. external API calls, database queries, etc.

A very simple example can be calling an external API and using the result later on in the application. 

## Implementation
```
package main

import (
    "fmt"
    "io/ioutil"
    "net/http"
)

type data struct {
    Body  []byte
    Error error
}

// this returns channel which contains future data
func futureData(url string) <-chan data {
    // buffer size 1 to make channel non-blocking
    c := make(chan data, 1)

    // goroutine to execute GET call to get data and put into channel
    go func() {
        resp, err := http.Get(url)
        if err != nil {
            c <- data{Body: []byte{}, Error: err}
        }

        defer resp.Body.Close()

        body, err := ioutil.ReadAll(resp.Body)
        c <- data{Body: body, Error: err}
    }()

    return c
}

func main() {
    future := futureData("https://jsonplaceholder.typicode.com/posts")

    /****************************
    ** Do all the other stuffs **
    ****************************/

    body := <-future
    fmt.Printf("response: %v", string(body.Body))
    fmt.Printf("error: %v", body.Error)
}
```

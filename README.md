gorilla/handlers
================
[![GoDoc](https://godoc.org/go.smantic.dev/handlers?status.svg)](https://godoc.org/github.com/gorilla/handlers)


Package handlers is a collection of handlers (aka "HTTP middleware") for use
with Go's `net/http` package (or any framework supporting `http.Handler`), including:

* [**LoggingHandler**](https://godoc.org/go.smantic.dev/handlers#LoggingHandler) for logging HTTP requests in the Apache [Common Log
  Format](http://httpd.apache.org/docs/2.2/logs.html#common).
* [**CombinedLoggingHandler**](https://godoc.org/go.smantic.dev/handlers#CombinedLoggingHandler) for logging HTTP requests in the Apache [Combined Log
  Format](http://httpd.apache.org/docs/2.2/logs.html#combined) commonly used by
  both Apache and nginx.
* [**CompressHandler**](https://godoc.org/go.smantic.dev/handlers#CompressHandler) for gzipping responses.
* [**ContentTypeHandler**](https://godoc.org/go.smantic.dev/handlers#ContentTypeHandler) for validating requests against a list of accepted
  content types.
* [**MethodHandler**](https://godoc.org/go.smantic.dev/handlers#MethodHandler) for matching HTTP methods against handlers in a
  `map[string]http.Handler`
* [**ProxyHeaders**](https://godoc.org/go.smantic.dev/handlers#ProxyHeaders) for populating `r.RemoteAddr` and `r.URL.Scheme` based on the
  `X-Forwarded-For`, `X-Real-IP`, `X-Forwarded-Proto` and RFC7239 `Forwarded`
  headers when running a Go server behind a HTTP reverse proxy.
* [**CanonicalHost**](https://godoc.org/go.smantic.dev/handlers#CanonicalHost) for re-directing to the preferred host when handling multiple 
  domains (i.e. multiple CNAME aliases).
* [**RecoveryHandler**](https://godoc.org/go.smantic.dev/handlers#RecoveryHandler) for recovering from unexpected panics.

Other handlers are documented [on the Gorilla
website](https://www.gorillatoolkit.org/pkg/handlers).

## Example

A simple example using `handlers.LoggingHandler` and `handlers.CompressHandler`:

```go
import (
    "net/http"
    "go.smantic.dev/handlers"
)

func main() {
    r := http.NewServeMux()

    // Only log requests to our admin dashboard to stdout
    r.Handle("/admin", handlers.LoggingHandler(os.Stdout, http.HandlerFunc(ShowAdminDashboard)))
    r.HandleFunc("/", ShowIndex)

    // Wrap our server with our gzip handler to gzip compress all responses.
    http.ListenAndServe(":8000", handlers.CompressHandler(r))
}
```

## License

BSD licensed. See the included LICENSE file for details.


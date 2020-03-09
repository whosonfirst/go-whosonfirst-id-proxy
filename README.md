# go-whosonfirst-id-proxy

An artisanal integer proxy service that implements the `go-whosonfirst-id` interface.

## Important

It's important to understand that the desired interfaces for artisanal integers, and their proxies, are not the same as those needed for Who's On First related operations, specifically exporting records. There is a lot of overlap but ultimately they are different which accounts for the sometimes confusing layer cake. We're trying to figure it out as we go...

## Example

_Error handling omitted for the sake of brevity._

### Simple

```
package main

import(
	"context"
	"fmt"	
	"github.com/aaronland/go-artisanal-integers-proxy"
	"github.com/aaronland/go-pool"
	"github.com/whosonfirst/go-whosonfirst-id-proxy/provider"	
)

func main () {

	ctx := context.Background()
	
	pl, _ := pool.NewPool(ctx, "memory://")

	svc_args := proxy.ProxyServiceArgs{
		BrooklynIntegers: true,
		MinCount:         10,
	}

	svc, _ := proxy.NewProxyServiceWithPool(pl, svc_args)
	pr, _ := provider.NewProxyServiceProvider(svc)

	id, _ := pr.NewID()
	fmt.Println(id)
}
```

### As an ID provider for `go-whosonfirst-export`

```
package main

import(
	"context"
	"github.com/aaronland/go-artisanal-integers-proxy"
	"github.com/aaronland/go-pool"
	"github.com/whosonfirst/go-whosonfirst-export"		
	"github.com/whosonfirst/go-whosonfirst-export/options"
	"github.com/whosonfirst/go-whosonfirst-id-proxy/provider"	
)

func main () {

	ctx := context.Background()
	
	pl, _ := pool.NewPool(ctx, "memory://")

	svc_args := proxy.ProxyServiceArgs{
		BrooklynIntegers: true,
		MinCount:         10,
	}

	svc, _ := proxy.NewProxyServiceWithPool(pl, svc_args)
	pr, _ := provider.NewProxyServiceProvider(svc)

	var body []byte // aka the thing you want to export

	opts, _ := options.NewDefaultOptionsWithProvider(pr)
	export.Export(body, opts, os.Stdout)	
}
```

## See also

* https://github.com/aaronland/go-artisanal-integers-proxy
* https://github.com/whosonfirst/go-whosonfirst-id/
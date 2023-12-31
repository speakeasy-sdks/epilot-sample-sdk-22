# Speakeasy Bar Starter SDK

<div align="left">
    <a href="https://speakeasyapi.dev/"><img src="https://custom-icon-badges.demolab.com/badge/-Built%20By%20Speakeasy-212015?style=for-the-badge&logoColor=FBE331&logo=speakeasy&labelColor=545454" /></a>
    
</div>

<br/>

This is a sample SDK generated for the Speakeasy Bar API. Have a look around and familiarize yourself with the Speakeasy Product!

When you're ready you can use this modify this repo to reference your own OpenAPI spec or go back to the Speakeasy app to complete onboarding and generate your first SDK!


## 🏗 **Welcome to your new SDK!** 🏗

It has been generated successfully based on your OpenAPI spec. However, it is not yet ready for production use. Here are some next steps:
- [ ] 🛠 Make your SDK feel handcrafted by [customizing it](https://www.speakeasyapi.dev/docs/customize-sdks)
- [ ] ♻️ Refine your SDK quickly by iterating locally with the [Speakeasy CLI](https://github.com/speakeasy-api/speakeasy)
- [ ] 🎁 Publish your SDK to package managers by [configuring automatic publishing](https://www.speakeasyapi.dev/docs/productionize-sdks/publish-sdks)
- [ ] ✨ When ready to productionize, delete this section from the README


### Local development

Once you have the SDK setup you may want to iterate on the SDK. Speakeasy supports OpenAPI vendor extensions that can be added to your spec to customize the SDK ergonomics (method names, namespacing resources etc.) and functionality (adding retries, pagination, multiple server support etc)

To get started install the Speakeasy CLI.

In your terminal, run:

```bash
brew install speakeasy-api/homebrew-tap/speakeasy
```
Once you annonate your spec with an extension you will want to run `speakeasy validate` to check the spec for correctness and `speakeasy generate` to recreate the SDK locally. More documentation on OpenAPI extensions [here](https://speakeasyapi.dev/docs/customize-sdks/namespaces/). Here's an example of adding a multiple server support to the spec so that your SDK supports production and sandbox versions of your API. The attached Makefile also attaches some helper commands.

```yaml
info:
  title: Example
  version: 0.0.1
servers:
  - url: https://prod.example.com # Used as the default URL by the SDK
    description: Our production environment
    x-speakeasy-server-id: prod
  - url: https://sandbox.example.com
    description: Our sandbox environment
    x-speakeasy-server-id: sandbox
```

Once you're finished iterating and happy with the output push only the latest version of spec into the repo and regenerate the SDK using step 6 above. 

<!-- Start SDK Installation [installation] -->
## SDK Installation

```bash
go get github.com/speakeasy-sdks/epilot-sample-sdk-22
```
<!-- End SDK Installation [installation] -->

<!-- Start SDK Example Usage [usage] -->
## SDK Example Usage

### Example

```go
package main

import (
	"context"
	templatespeakeasybar "github.com/speakeasy-sdks/template-speakeasy-bar"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/shared"
	"log"
)

func main() {
	s := templatespeakeasybar.New()

	var drinkType *shared.DrinkType = shared.DrinkTypeSpirit

	ctx := context.Background()
	res, err := s.Drinks.ListDrinks(ctx, drinkType)
	if err != nil {
		log.Fatal(err)
	}

	if res.Classes != nil {
		// handle response
	}
}

```
<!-- End SDK Example Usage [usage] -->

<!-- Start Available Resources and Operations [operations] -->
## Available Resources and Operations

### [Authentication](docs/sdks/authentication/README.md)

* [Authenticate](docs/sdks/authentication/README.md#authenticate) - Authenticate with the API by providing a username and password.

### [Drinks](docs/sdks/drinks/README.md)

* [GetDrink](docs/sdks/drinks/README.md#getdrink) - Get a drink.
* [ListDrinks](docs/sdks/drinks/README.md#listdrinks) - Get a list of drinks.

### [Ingredients](docs/sdks/ingredients/README.md)

* [ListIngredients](docs/sdks/ingredients/README.md#listingredients) - Get a list of ingredients.

### [Orders](docs/sdks/orders/README.md)

* [CreateOrder](docs/sdks/orders/README.md#createorder) - Create an order.

### [Config](docs/sdks/config/README.md)

* [SubscribeToWebhooks](docs/sdks/config/README.md#subscribetowebhooks) - Subscribe to webhooks.
<!-- End Available Resources and Operations [operations] -->

<!-- Start Error Handling [errors] -->
## Error Handling

Handling errors in this SDK should largely match your expectations.  All operations return a response object or an error, they will never return both.  When specified by the OpenAPI spec document, the SDK will return the appropriate subclass.

| Error Object       | Status Code        | Content Type       |
| ------------------ | ------------------ | ------------------ |
| sdkerrors.APIError | 5XX                | application/json   |
| sdkerrors.SDKError | 4xx-5xx            | */*                |

### Example

```go
package main

import (
	"context"
	"errors"
	templatespeakeasybar "github.com/speakeasy-sdks/template-speakeasy-bar"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/operations"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/sdkerrors"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/shared"
	"log"
)

func main() {
	s := templatespeakeasybar.New(
		templatespeakeasybar.WithSecurity("<YOUR_API_KEY_HERE>"),
	)

	ctx := context.Background()
	res, err := s.Authentication.Authenticate(ctx, operations.AuthenticateRequestBody{})
	if err != nil {

		var e *sdkerrors.APIError
		if errors.As(err, &e) {
			// handle error
			log.Fatal(e.Error())
		}

		var e *sdkerrors.SDKError
		if errors.As(err, &e) {
			// handle error
			log.Fatal(e.Error())
		}
	}
}

```
<!-- End Error Handling [errors] -->

<!-- Start Server Selection [server] -->
## Server Selection

### Select Server by Name

You can override the default server globally using the `WithServer` option when initializing the SDK client instance. The selected server will then be used as the default on the operations that use it. This table lists the names associated with the available servers:

| Name | Server | Variables |
| ----- | ------ | --------- |
| `prod` | `https://speakeasy.bar` | None |
| `staging` | `https://staging.speakeasy.bar` | None |
| `customer` | `https://{organization}.{environment}.speakeasy.bar` | `environment` (default is `prod`), `organization` (default is `api`) |

#### Example

```go
package main

import (
	"context"
	templatespeakeasybar "github.com/speakeasy-sdks/template-speakeasy-bar"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/operations"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/shared"
	"log"
)

func main() {
	s := templatespeakeasybar.New(
		templatespeakeasybar.WithServer("customer"),
		templatespeakeasybar.WithSecurity("<YOUR_API_KEY_HERE>"),
	)

	ctx := context.Background()
	res, err := s.Authentication.Authenticate(ctx, operations.AuthenticateRequestBody{})
	if err != nil {
		log.Fatal(err)
	}

	if res.Object != nil {
		// handle response
	}
}

```

#### Variables

Some of the server options above contain variables. If you want to set the values of those variables, the following options are provided for doing so:
 * `WithEnvironment templatespeakeasybar.ServerEnvironment`
 * `WithOrganization string`

### Override Server URL Per-Client

The default server can also be overridden globally using the `WithServerURL` option when initializing the SDK client instance. For example:
```go
package main

import (
	"context"
	templatespeakeasybar "github.com/speakeasy-sdks/template-speakeasy-bar"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/operations"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/shared"
	"log"
)

func main() {
	s := templatespeakeasybar.New(
		templatespeakeasybar.WithServerURL("https://speakeasy.bar"),
		templatespeakeasybar.WithSecurity("<YOUR_API_KEY_HERE>"),
	)

	ctx := context.Background()
	res, err := s.Authentication.Authenticate(ctx, operations.AuthenticateRequestBody{})
	if err != nil {
		log.Fatal(err)
	}

	if res.Object != nil {
		// handle response
	}
}

```
<!-- End Server Selection [server] -->

<!-- Start Custom HTTP Client [http-client] -->
## Custom HTTP Client

The Go SDK makes API calls that wrap an internal HTTP client. The requirements for the HTTP client are very simple. It must match this interface:

```go
type HTTPClient interface {
	Do(req *http.Request) (*http.Response, error)
}
```

The built-in `net/http` client satisfies this interface and a default client based on the built-in is provided by default. To replace this default with a client of your own, you can implement this interface yourself or provide your own client configured as desired. Here's a simple example, which adds a client with a 30 second timeout.

```go
import (
	"net/http"
	"time"
	"github.com/myorg/your-go-sdk"
)

var (
	httpClient = &http.Client{Timeout: 30 * time.Second}
	sdkClient  = sdk.New(sdk.WithClient(httpClient))
)
```

This can be a convenient way to configure timeouts, cookies, proxies, custom headers, and other low-level configuration.
<!-- End Custom HTTP Client [http-client] -->

<!-- Start Special Types [types] -->
## Special Types


<!-- End Special Types [types] -->



<!-- Start Authentication [security] -->
## Authentication

### Per-Client Security Schemes

This SDK supports the following security scheme globally:

| Name     | Type     | Scheme   |
| -------- | -------- | -------- |
| `APIKey` | apiKey   | API key  |

You can configure it using the `WithSecurity` option when initializing the SDK client instance. For example:
```go
package main

import (
	"context"
	templatespeakeasybar "github.com/speakeasy-sdks/template-speakeasy-bar"
	"github.com/speakeasy-sdks/template-speakeasy-bar/pkg/models/operations"
	"log"
)

func main() {
	s := templatespeakeasybar.New(
		templatespeakeasybar.WithSecurity("<YOUR_API_KEY_HERE>"),
	)

	ctx := context.Background()
	res, err := s.Authentication.Authenticate(ctx, operations.AuthenticateRequestBody{})
	if err != nil {
		log.Fatal(err)
	}

	if res.Object != nil {
		// handle response
	}
}

```
<!-- End Authentication [security] -->

<!-- Placeholder for Future Speakeasy SDK Sections -->

# Development

## Maturity

This SDK is in beta, and there may be breaking changes between versions without a major version update. Therefore, we recommend pinning usage
to a specific package version. This way, you can install the same version each time without breaking changes unless you are intentionally
looking for the latest version.

## Contributions

While we value open-source contributions to this SDK, this library is generated programmatically.
Feel free to open a PR or a Github issue as a proof of concept and we'll do our best to include it in a future release!

### SDK Created by [Speakeasy](https://docs.speakeasyapi.dev/docs/using-speakeasy/client-sdks)

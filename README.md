# htmx for Rails

Add support for partial responses for htmx in Ruby on Rails controllers.

![fullstack-1](https://github.com/guilleiguaran/rails-htmx/assets/160941/8b9307af-111a-4af0-92de-218aac07797f)


## Installation

Install the gem and add it to the application's Gemfile by executing:

    $ bundle add rails-htmx


## Installing htmx

Providing the assets for htmx is out of the scope of this library and
the assets gems have been deprecated (along with Sprockets) by Rails.

Instead is recommended to install htmx from the official sources:

### Via CDN

Add the script to your application.html.erb layout file:

```html
<script src="https://unpkg.com/htmx.org@1.9.2" integrity="sha384-L6OqL9pRWyyFU3+/bjdSri+iIphTN/bvYyM37tICVyOJkWZLpP2vGn6VUEXgzg6h" crossorigin="anonymous"></script>
```

Check the [htmx docs](https://htmx.org/docs/#installing) to make sure that you're using the latest
version of the package

### Using a JS Bundler (e.g jsbundling-rails or webpacker)

Add the htmx.org package using Yarn:

```bash
yarn add htmx.org
```

### Using Import Maps (e.g importmap-rails)

Pin the dependency to the into the importmap:

```bash
bin/importmap pin htmx.org
```


Note: You might want to disable Turbo (and turbo-rails) if your application has it
already installed.


## Usage

By default, the `rails-htmx` prevents the render of the application layout in
your controllers and instead returns only the yielded view for the
requests that have the `HX-Request` header present.

Additionally, `rails-htmx` modify redirects in non-GET and non-POST requests to
return 303 (See Other) as status code instead of 302 to handle XHR redirects correctly.

For more information about how to use htmx please consult the [htmx docs](https://htmx.org/docs/).

### Preventing htmx requests

In certain cases might want to return a full view (e.g in a login page) instead
of a partial yielded view, you can do this calling the prevent_htmx!
method in your controller actions or in an callback:

```ruby
  before_action :prevent_htmx!, if: -> { some_condition? }

  def login
    prevent_htmx!
    # ...
  end
```

### Custom Layouts

If you have additional content you want to always be returned with your htmx requests,
you can override `htmx_layout` in your controller and specify a layout to render
(by default, it's `false`)

```ruby
class ApplicationController < ActionController::Base
  def htmx_layout
    'htmx'
  end
end
```

## Tips

Those are some tips for using htmx with Rails.

### Add the CSRF Token to the htmx requests

Add the `X-CSRF-Token` to the `hx-headers` attributes in your `<body>` tag so it's added by
default in XHR requests done by htmx:

```erb
<body hx-headers='{"X-CSRF-Token": "<%= form_authenticity_token %>"}'>
```

### Using the data prefix

You can use the `data-` prefix to make easier adding the htmx attributes with the Rails helpers:

```erb
<%= link_to "Home", root_path, data: { "hx-swap": "outerHTML" } %>
```

### Boosting Rails helpers

You can use the default Rails helpers without modifications in your markup with the htmx
[Boosting feature](https://htmx.org/docs/#boosting):

```erb
<div hx-boost="true">
  <%= link_to "New post", new_post_path %>
</div>
```

## License
rails-htmx is released under the [MIT License](https://opensource.org/license/mit/).

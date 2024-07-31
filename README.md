# Sakura

![Static Badge](https://img.shields.io/badge/build-unknown-pink)
![Static Badge](https://img.shields.io/badge/version-0.1.7-pink)


A webview GUI based on [xiejiangzhi/webview](https://github.com/xiejiangzhi/webview) which is based on [zserge/webview](https://github.com/zserge/webview)


## Requirements

* `golang`
* `Cocoa`/`WebKit` on macOS, `gtk-webkit2` on Linux and `MSHTML` (IE10/11) on Windows.


## Installation

Add this line to your application's Gemfile:

```ruby
gem 'webview'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install webview

## Usage

```ruby
app = Webview::App.new
app.open('http://localhost:3000?foo=bar')

at_exit { app.close }
begin
  app.join
rescue Interrupt
  app.close
end
```

Allow debug page

```ruby
Webview::App.new(debug: true).open('http://example.com')
```

All options

```ruby
Webview::App.new(title: 'ttt', width: 600, height: 400, resizable: true, debug: true)
```

Run with your backend.

```ruby
app = Webview::App.new
backend = Thread.new { `rails s` }
app.open('http://localhost:3000')

at_exit { app.close }
begin
  app.join
rescue Interrupt
  app.close
  backend.kill
end
```

RPC with Javascript

```javascript
window.rpc_cb = function(type, value, user_data) {
  consloe.log(type, value, user_data)
}

var type = 'openfile'
window.external.invoke(type + ',' + 'my_data_id');
// Open a dialog. and call rpc_cb('openfile', 'selected_file_path', "my_data_id") after user choice file.

window.external.invoke('invalid_type' + ',' + 'my_data');
// if got a error, will call rpc_cb('error', 'invalid_type', 'my_data')
```

## Javascript RPCs

* close: close this app window, no callback
* fullscreen: no callback
* unfullscreen: no callback
* openfile: Open a dialog, and call rpc_cb('openfile', path, user_data) 
* opendir: Open a dialog, and call rpc_cb('opendir', path, user_data) 
* savefile: Open a dialog, and call rpc_cb('savefile', path, user_data) 


## TODO

i don't know.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/OfficialSeafoam/sakura.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

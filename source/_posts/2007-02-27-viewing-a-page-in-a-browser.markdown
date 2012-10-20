---
layout: post
---
I've been doing a bunch of integration testing of a rails site lately.  I have found it quite useful to be able to quickly view the current page in a browser.

I added the following method to my integration testing DSL:

``` ruby
module IntegrationDsl

  def view
    filename = File.dirname(__FILE__) + "/integration/.integration_test_output_for_browser.html"
    File.open(filename, "w+") { | file | file.write(response.body) }
    `open #{filename}`
  end

end
```

It simply writes out the response body to a file and then opens it (using the \`open\` on a mac).

So now in my integration test, I can view the current page in a browser like this:

``` ruby
def test_feature
  s = new_session_as :duff
  s.view
end
```

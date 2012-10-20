---
layout: post
---
There have been times when integration testing a rails site that I've wanted to click a link on a page without caring about it's route or url.

So let's say the following exists on a page in my app:

``` html
<html>
  <a href="http://meresheep.com/users/4;resend_activation_email">
      resend your activation email
  </a>
</html>
```

In my integration test, I can now do things like this:

``` ruby
def test_non_activated_user
  s = new_session_as :duff
  s.click_link("resend your activation email")
  s.assert_template("invitation_sent")
end
```

To get this functionality, I added the following method to my integration testing DSL:
``` ruby
module IntegrationDsl

  def click_link(link_text)
    assert_select("a", link_text, "Trying to click a link named '#{link_text}' that did not exist") do | links |
      get_via_redirect links.first.attributes['href']
    end
  end

end
```

The test fails if the link doesn't exist on the current page.

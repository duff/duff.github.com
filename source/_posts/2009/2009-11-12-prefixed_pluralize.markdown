---
layout: post
title: "Prefixed Pluralize"
---

I'd like the view to be smart and say something like "There are 3 plans available" or "There is 1 plan available" based on the number of plans.

Icky:
``` ruby
%p There #{( current_gym.membership_plans.count == 1 ) ? "is" : "are"} #{pluralize(current_gym.membership_plans.count, "membership plan")}.
```

Better:
``` ruby
%p There #{prefixed_pluralize(current_gym.membership_plans.count, "is", "are", "membership plan")}.
```

Here's the little helper method:
``` ruby
def prefixed_pluralize(count, singular_prefix, plural_prefix, singular_suffix, plural_suffix = nil)
  prefix = (count == 1) ? singular_prefix : plural_prefix
  "#{prefix} #{pluralize(count, singular_suffix, plural_suffix)}"
end
```

And the test:
``` ruby
test "prefixed_pluralize" do
  assert_equal "are 7 dogs", prefixed_pluralize(7, "is", "are", "dog")
  assert_equal "is 1 dog", prefixed_pluralize(1, "is", "are", "dog")
  assert_equal "are 0 animals", prefixed_pluralize(0, "is", "are", "animals")
  assert_equal "are 5 users", prefixed_pluralize(5, "is", "are", "person", "users")
  assert_equal "are now 2 penguins", prefixed_pluralize(2, "is now", "are now", "penguin")
end
```

Helpful?

---
layout: post
---
I've got a pretty simple test: 

{% highlight ruby linenos %}
def test_relationships
  wellspring = churches(:wellspring)
  cleaning_crew = Group.create!(:name => "Cleaning Crew", :church => wellspring)
  assert_equal([cleaning_crew], wellspring.groups)
end
{% endhighlight %}

The problem is that when this test fails, I get a ridiculous amount of output and it's difficult to quickly determine why the test is failing.  Here's the output I get:

{% highlight bash %}
1) Failure:
test_relationships:10
<[#<Group:0x3492918
  @attributes=
   {"name"=>"Cleaning Crew",
    "church_id"=>1,
    "id"=>32,
at top level in "created_at"=>Thu Mar 01 15 at line 56
  @church=
at top level in #<Church at line 0
    @attributes=
     {"name"=>"Wellspring Community Church",
      "account_owner_id"=>"1",
      "timezone"=>"Alaska",
      "subdomain"=>"wellspring",
      "id"=>"1",
      "tz_name"=>nil},
    @groups=
at top level in #<Group at line 0
  @errors=
at top level in #<ActiveRecord::Errors at line 0
  @new_record=false>]> expected but was
<[#<Group:0x348c0a4 @attributes={"name"=>"Elders", "church_id"=>"1", "id"=>"1", "created_at"=>"2007-03-01 10:01:58"}>, #<Group:0x348c07c @attributes={"name"=>"Children's Ministry", "church_id"=>"1", "id"=>"4", "created_at"=>"2007-03-01 10:01:58"}>, #<Group:0x348c054 @attributes={"name"=>"Cleaning Crew", "church_id"=>"1", "id"=>"32", "created_at"=>"2007-03-01 15:56:38"}>]>.

1 tests, 1 assertions, 1 failures, 0 errors
{% endhighlight %}

This is what I'd like the output to look like:

{% highlight bash %}
 1) Failure:
test_relationships(GroupTest)
method assert_arrays_equal in test_unit_extras.rb at line 8
method test_relationships in group_test.rb at line 11
<"Cleaning Crew"> expected but was
<"Elders, Children's Ministry, Cleaning Crew">.

1 tests, 1 assertions, 1 failures, 0 errors
{% endhighlight %}

To me, that's a bit easier to read.  Here's how I did it.  The test has a small change.  Instead of calling assert_equal, it calls assert_arrays_equal.  So the test now looks like this:

{% highlight ruby linenos %}
def test_relationships
  wellspring = churches(:wellspring)
  cleaning_crew = Group.create!(:name => "Cleaning Crew", :church => wellspring)
  assert_arrays_equal([cleaning_crew], wellspring.groups, :name)
end
{% endhighlight %}

Where did assert_arrays_equal come from?  You can get it by installing the plugin I just created:

{% highlight bash %}
./script/plugin install git://github.com/duff/assert_arrays_equal.git
{% endhighlight %}

assert_arrays_equal allows you to specify which method you'd like to be called on the objects in your arrays for the output message.  It still asserts that the arrays are actually equal.  The failure error message now uses the method you specify.  In the test above, I specified the :name method because I want Group.#name to be called rather than Group#inspect.  If you don't specify a method, it defaults to :to_s.  

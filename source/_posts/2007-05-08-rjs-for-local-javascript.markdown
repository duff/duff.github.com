---
layout: post
---
There are times when I need to write a local javascript function (without needing a round trip to the server) and I'd like to use RJS rather than writing the function in pure javascript.  Why would I want to use RJS?  

* The syntax is clean and it's often less code than pure javscript. 
* When writing this function, I can mix in powerful Ruby constructs. 
* It's consistent with the other RJS in the project.
* I automatically get the function wrapped up in a CDATA tag.
* I automatically get exceptions handled by giving me an alert of the problem.
* It's easy to conditionally exclude some of the javascript.
* My rails views look cleaner.

Here's an example from a rails view (new.erb):

{% highlight ruby linenos %}
<%= define_js_function(:my_cool_local_function) do | page | 
		page[:choice_entry_area].hide unless @question.multiple_choice?
		page[:add_answer_link].show
		page[:add_answer_button].hide
	end	
%> 
{% endhighlight %}

So where'd this define_js_function method come from?  I added it to ApplicationHelper.  Here it is:

{% highlight ruby linenos %}
def define_js_function(function_name, &block)
  parens = function_name.kind_of?(Symbol) ? "()" : ""
  update_page_tag do | page |
    page << "function #{function_name}#{parens} {"
    yield page
    page << "}"
  end	
end
{% endhighlight %}

When the view is rendered in the browser, the following javascript gets generated:
{% highlight html %}
<script type="text/javascript">
//<![CDATA[
try {
function my_cool_local_function() {
$("choice_entry_area").hide();
$("add_answer_link").show();
$("add_answer_button").hide();
}
} catch (e) { alert('RJS error:\n\n' + e.toString()); alert('function my_cool_local_function() {\n$(\"choice_entry_area\").hide();\n$(\"add_answer_link\").show();\n$(\"add_answer_button\").hide();\n}'); throw e }
//]]>
</script>
{% endhighlight %}


If the javascript function you're creating takes no arguments, then you simply pass a symbol to define_js_function to specify the function name.  If your function takes arguments, then you can pass in a string which contains those arguments like so:

{% highlight ruby linenos %}
<%= define_js_function("add_tag(tag)") do | page | 
		page << "$('tag_list_str').value += tag"
	end	
%>
{% endhighlight %}

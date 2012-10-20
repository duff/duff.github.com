---
layout: post
---
Running \`cap deploy:pending\` displays the commits since your last deploy.  I wanted it's output to be a bit different primarily so I could send a message to the others on the team about what's just been deployed.

Specifically, I wanted the output to look something like this:

{% highlight text %}

Deployment revision 49c45b7
I deployed the latest. It includes:

* Added avatar support  (Duff)
* Cleaned up the project switcher  (Alex)
{% endhighlight %}

To do that, I added the following capistrano recipe:



{% highlight ruby %}

namespace :deploy do
  namespace :pending do
    desc <<-DESC
      Show the commits since the last deploy
    DESC
    task :default, :except => { :no_release => true } do
      deployed_already = current_revision
      to_be_deployed = `git rev-parse --short "HEAD"`

      puts "\n\nDeployment revision #{to_be_deployed}"
      puts "I deployed the latest. It includes:"
      puts
      system(%Q{git log --no-merges --pretty=format:"* %s %b (%cn)" #{deployed_already}.. | replace '<unknown>' ''})
      puts "\n\n\n"
    end
  
  end
end
{% endhighlight %}

Then I added a hook in my deploy.rb to run this automatically whenever someone deploys:

{% highlight ruby %}
before "deploy:update_code", "deploy:pending:default" 
{% endhighlight %}


The next piece of automation I'd like is to have it automatically post a message to [basecamp](http://basecamphq.com/).

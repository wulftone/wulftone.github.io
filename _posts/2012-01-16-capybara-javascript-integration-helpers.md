---
layout: post
title: Capybara JavaScript Integration Helpers
date: 16/01/2012
categories: ruby rspec
---

## Waiting for Ajax

I needed a quick way to wait for ajax to finish in my capybara-based tests, so I made a quick google search.  I discovered that pivotallabs had the same issue, and used the following code:

{% highlight ruby %}
page.wait_until do
  page.evaluate_script('$.active') == 0
end
{% endhighlight %}

That's all well and good, but I didn't really want to type that out every single time, so I made a helper!

{% highlight ruby %}
module IntegrationHelpers
  def wait_for_ajax
    page.wait_until do
      page.evaluate_script('$.active') == 0
    end
  end
end
{% endhighlight %}

I just make sure to include it in whatever `*_spec.rb` file I need to use it in.  If you're using cucumber, there are other ways to include it in `World`, I believe.

Here's an example:

{% highlight ruby %}
require 'spec_helper' # the 'faker' gem is required in my spec_helper.rb
feature 'Homefront Homepage', js: true do
  include IntegrationHelpers
  background do
    @user = FactoryGirl.create :user
  end
  scenario 'I\'ve already signed up and just logged in' do
    visit root_path
    page.should have_content 'Sign in to my awesome app'
    fill_in 'Email', with: @user.email
    fill_in 'Password', with: 'xxxxxx'
    click_button 'Sign in'
    page.should have_content 'You are logged in'
  end
  scenario 'I should be able to edit my user info' do
    click_link 'My Profile'
    page.should have_selector 'form#user-form'
    fill_in 'First name', with: first_name = Faker::Name.first_name
    fill_in 'Last name', with: last_name = Faker::Name.last_name
    fill_in 'Birthdate', with: birthdate = Date.yesterday
    choose 'F'
    click_button 'Update'
    wait_for_ajax # here's where the magic "waiting" happens!
    @user.reload.first_name.should eq first_name
    @user.last_name.should eq last_name
    @user.birthdate.should eq birthdate
    @user.gender.should eq 'f'
  end
end
{% endhighlight %}

Awesome! My spec now waits patiently for all ajax on the faux-page to complete.

## Backbone.js back helper

Upon further integration testing, I quickly discovered yet another need: a way to press "back" like a user would do quite naturally, even habitually... When I'm actually *using* my app, I sometimes pretend to be a click-happy dufus user and click all kinds of things in as many wrong ways as possible, and the back button is a great one.  Pressing backspace when you're not in a text field triggers it, as does one of those extra buttons on my mouse, and of course there's the regular left-clicking it on the browser action bar.  Integration testing a backbone.js app, which this one is, makes it almost a requirement to test.  As it turns out, it's really simple with browsers that support `window.history`:

{% highlight javascript %}
page.evaluate_script('window.history.back()')
{% endhighlight %}

Bam! Done.  Throw it in the module I made before:

{% highlight ruby %}
module IntegrationHelpers
  def wait_for_ajax
    page.wait_until do
      page.evaluate_script('$.active') == 0
    end
  end
  def back
    page.evaluate_script('window.history.back()')
  end
end
{% endhighlight %}

Use it!

{% highlight ruby %}
scenario 'I should be able to edit my user info' do
  click_link 'My Profile'
  page.should have_selector 'form#user-form'
  fill_in 'First name', with: first_name = Faker::Name.first_name
  fill_in 'Last name', with: last_name = Faker::Name.last_name
  fill_in 'Birthdate', with: birthdate = Date.yesterday
  choose 'F'
  click_button 'Update'
  wait_for_ajax
  @user.reload.first_name.should eq first_name
  @user.last_name.should eq last_name
  @user.birthdate.should eq birthdate
  @user.gender.should eq 'f'
  back # OMG I can go backwards in a Capybara test!
  page.should have_content 'Welcome to AwesomeApp!'
end
{% endhighlight %}

Sweet.

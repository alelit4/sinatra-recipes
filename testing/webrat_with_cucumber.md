Features from Cucumber and Webrat 
---------------------------------


**A Feature Example**

    Feature: View my page
      In order for visitors to feel welcome
      We must go out of our way
      With a kind greeting

      Scenario: My page
        Given I am viewing my page 
        Then I should see "Welcome to my page!"

**Step to it**

    Given /^I am viewing my page$/ do
      visit('/')
    end

    Then /^I should see "([^"]*)"$/ do |text|
       last_response.body.should match(/#{text}/m)
    end

**env.rb**

    ENV['RACK_ENV'] = 'test'

    require 'rubygems'
    require 'rack/test'
    require 'rspec/expectations'
    require 'webrat'

    begin 
      require_relative '../../my-app.rb'
    rescue NameError
      require File.expand_path('../../my-app.rb', __FILE__)
    end

    Webrat.configure do |config|
      config.mode = :rack
    end

    class WebratMixinExample
      include Rack::Test::Methods
      include Webrat::Methods
      include Webrat::Matchers

      Webrat::Methods.delegate_to_session :response_code, :response_body

      def app
        Sinatra::Application
      end
    end

    World{WebratMixinExample.new}

**Cucumber Resources**

*   [Cucumber Homepage](http://cukes.info/)
*   [Source on github](https://github.com/aslakhellesoy/cucumber)
*   [Documentation](https://github.com/aslakhellesoy/cucumber/wiki/)


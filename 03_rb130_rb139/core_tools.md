# Core Ruby Tools
[Reference](https://launchschool.com/books/core_ruby_tools/read/introduction)

## Section Links
[Summary](#summary)\
[Gems](#gems)\
[Ruby Version Managers](#ruby-version-managers)\
[Bundler](#bundler)\
[Rake](#rake)\
[Hands-on Walkthrough](#handson-walkthrough)


## Summary
- Ruby projects are programs and libraries that make use of Ruby as the primary development language. Each Ruby project is typically designed to use a specific version (or versions) of Ruby, and may also use a variety of different RubyGems aka Gems.

- **Gems** are packages of code that we can download, install, and use in our Ruby programs or from the command line.

- Due to differing features between different Ruby versions and backward compatibility issues, we may need different Ruby versions to maintain different applications.

- Ruby Version Manager provides a convenient way to maintain multiple installations of Ruby and accompanying tools (e.g. `irb` and `gem`) 

- Within each version of Ruby, we can house multiple Gems. Each Gem becomes accessible when the Ruby version it is installed under is used. To run a Gem in multiple versions of Ruby, it has to be installed under all Ruby versions we want to use it with.

- It is common to house different versions of a Gem under a particular Ruby version. This occurs naturally as we install updated Gems, but can also be a project requirement: we need one Gem version for one project but another for other projects.

- While Ruby version managers are useful at helping us switch between Ruby versions, the **Bundler** gem is better suited to resolving Gem dependencies and finding compatible versions based on the requirements specified in `Gemfile` for a Ruby project. `bundle exec` also helps ensure we run the correct Gem versions when multiple versions are available.

- **Rake**, another Gem, is useful tool to help us **automate repetitive development tasks**. This is done by creating varied tasks needed for a project in a `Rakefile`, from running tests, building databases, packaging to releasing the software, etc. The code in a Rakefile is essentially Ruby but make use of methods such as `desc` and `task` written in Domain Specific Language (DSL). The tasks that Rake performs are varied, and frequently change from one project to another; we use the `Rakefile` file to control which tasks our project needs.

[Back to Top](#section-links)


## Gems
- RubyGems, or Gems for short, are packages of code that we can download, install, and use in our Ruby programs or from the command line. The `gem` command manages our Gems; all versions of Ruby since version 1.9 supply `gem` as part of the standard installation. Common gems we used so far include:
	- `rubocop`
	- `pry`
	- `bundler`
	- `rake`

- We use `gem install GemName` to install a gem. The gem files will be install in the subdirectory of the currently active Ruby version. 
- To determine where `gem` puts things in our system, we can run `gem env` 

[Back to Top](#section-links)


## Ruby Version Managers
- Ruby version managers let us **manage multiple versions of Ruby, associated utilities (e.g. irb) and RubyGems**. With version managers, we can install and uninstall ruby versions and gems, and run specific versions of ruby with specific programs and environments.

- The two main version managers, **RVM and rbenv**, are similar in function, with little to distinguish between the two for most developers. By default, RVM has more features, but rbenv plugins provide much of the functionality not provided by the base install of rbenv. RVM works by dynamically managing our environment, mostly by modifying our `PATH` variable and replacing the built-in `cd` command with an RVM-aware shell function; rbenv works by just modifying our `PATH` and some other environment variables.

[Back to Top](#section-links)


## Bundler
- Ruby programs often need a specific version of Ruby, and specific versions of the Gems it uses. While Ruby version managers can take care of most of the issues arising from these differing requirements, sometimes they aren't enough. For example, we may need to use Ruby 2.2.2 for two different projects instead of our default 2.3.1, but we may also need separate versions of the Rails Gem, say 4.2.7 for one project, and version 5.0.0 for the other. While both RVM and rbenv (with the aid of a plugin) can handle these requirements, the easier and more common path is to use a RubyGem called Bundler. 

- Bundler allows us to describe exactly which Ruby and Gems we want to use with our Ruby apps. Specifically, it lets us install multiple versions of each Gem under a specific version of Ruby and then use the proper version in our app.

- Bundler is a RubyGem, so we must install it like a normal Gem: `gem install bundler`.

- To use Bundler, we provide a file named `Gemfile` that describes the Ruby and Gem versions we want for our app. We use a DSL described on the [Bundler website](http://bundler.io/v1.13/gemfile.html) to provide this information.

- Bundler uses the `Gemfile` to generate a `Gemfile.lock` file via the `bundle install` command. `Gemfile.lock` describes the actual versions of each Gem that our app needs, including any Gems that the Gems listed in `Gemfile` depend on. The `bundler/setup` package tells our Ruby program to use `Gemfile.lock` to determine which Gem versions it should load.

- Once Bundler creates our `Gemfile.lock`, add `require 'bundler/setup'` to the beginning of our app, before any other Gems. This is **not needed** if our app is a Rails app.

- The `bundle exec` command ensures that executable programs installed by Gems don't interfere with our app's requirements. For instance, if our app needs a specific version of `rake` but the default version of `rake` differs, `bundle exec` ensures that we can still run the specific `rake` version compatible with our app.

[Back to Top](#section-links)


## Rake
Rake is a Rubygem that automates many common functions required to build, test, package, and install programs; it is already bundled within every modern Ruby installation, so no separate installation is needed.

Here are some common Rake tasks that we may encounter:

- Set up required environment by creating directories and files
- Set up and initialize databases
- Run tests
- Package your application and all of its files for distribution
- Install the application
- Perform common Git tasks
- Rebuild certain files and directories (assets) based on changes to other files and directories

**Sample Rakefile**
```ruby
desc 'Say hello'
task :hello do
  puts "Hello there. This is the `hello` task."
end

desc 'Say goodbye'
task :bye do
  puts 'Bye now!'
end

desc 'Do everything'
task :default => [:hello, :bye]
```

Terminal
```terminal
$ bundle exec rake -T
rake bye      # Say goodbye
rake default  # Do everything
rake hello    # Say hello
```

Execution
```terminal
$ bundle exec rake bye
Bye now!

$ bundle exec rake hello
Hello there. This is the `hello` task.

$ bundle exec rake default
Hello there. This is the `hello` task.
Bye now!

$ bundle exec rake                     # we don't need to specify 'default'
Hello there. This is the `hello` task.
Bye now!
```
Note: It is preferred to execute `rake` prepended with `bundle exec` if bundler is used for the project to ensure the correct version of `rake` that our application depends on is used. 

[Back to Top](#section-links)


## Hands-on WalkThrough

### 1. Create Project Structure
Here we create a project directory and git repository. This directory should not be within an existing git repository to avoid nested repositories
```shell
mkdir todolist_project
cd todolist_project

echo "# todolist_project" >> README.md
git init
git add README.md
git commit -m "Initial commit"
git branch -M main
git remote add origin git@github.com:USERNAME/todolist_project.git
git push -u origin main
```

Within the project directory, we include a `readme.md` file, and `lib` and `test` directories for source codes. Web based programs tend to also have an `assets` directory with `images`, `javascript` and `stylesheets` subdirectories and a `views` directory for HTML template files.

**Current project file structure**
```plaintext
todolist_project
├── README.md
├── lib
│   └── todolist_project.rb
└── test
    └── todolist_project_test.rb
```


### 2. Create Gemfile
Next we create a gemfile that lists the dependencies of this project. First, we list the official source where most projects find their Rubygems. Then, the Ruby version required for the project. Followed by the gems needed by the source files. In this case, our source files in `lib` and `test` have the following `require`s:
- `require 'minitest/autorun'`
- `require 'minitest/reporters'`

These corresponds to "minitest" and "minitest-reporters" Gems. A search for files `minitest/autorun.rb` and `minitest/reporters.rb` on developer's system found them in `...gems/minitest-5.10.1/lib` and `...gems/minitest-reporters-1.1.8/lib` directories repectively. We can use the versions there for the gemfile. The `~> 5.10` tells Bundler we want to use a version of minitest that is at least 5.10 or later. Using `~>` is helpful to avoid compatibility issues.
```ruby
source 'https://rubygems.org'

ruby '2.5.0'

gem 'minitest', '~> 5.10'
gem 'minitest-reporters', '~> 1.1'
```

Next tell the Ruby version manager to use Ruby 2.5.0 and install bundler if not yet installed.

**rbenv**
```terminal
$ rbenv install 2.5.0     # if not yet installed on system
$ rbenv local 2.5.0
$ gem install bundler     # if not yet installed on system
```

**rvm**
```terminal
rvm install ruby-2.5.0    # if not yet installed on system
$ rvm use 2.5.0
$ gem install bundler     # if not yet installed on system
```

### 3. Run Bundler
Once `Gemfile` is complete, tell Bundler to find and install all project dependencies.
```terminal
$ bundle install
Fetching gem metadata from https://rubygems.org/..................
Resolving dependencies...
Using ansi 1.5.0
Using builder 3.2.4
Using bundler 2.1.4
Fetching minitest 5.14.0
Installing minitest 5.14.0
Using ruby-progressbar 1.10.1
Using minitest-reporters 1.4.2
Bundle complete! 2 Gemfile dependencies, 6 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
```

`bundle install` also creates a `Gemfile`.lock that contains all the dependency information. Whenever we update our `Gemfile`, we should rerun `bundle install` to update the `Gemfile.lock`
```plaintext
GEM
  remote: https://rubygems.org/
  specs:
    ansi (1.5.0)
    builder (3.2.4)
    minitest (5.14.0)
    minitest-reporters (1.4.2)
      ansi
      builder
      minitest (>= 5.0)
      ruby-progressbar
    ruby-progressbar (1.10.1)

PLATFORMS
  ruby

DEPENDENCIES
  minitest (~> 5.10)
  minitest-reporters (~> 1.1)

RUBY VERSION
   ruby 2.5.0

BUNDLED WITH
   2.1.4
```

Next, we need to add `require 'bundler/setup'` before any `require` statements in our source files since this is not a Rails app. `bundler/setup` first removes all Gem directories from Ruby's `$LOAD_PATH` global array. Ruby uses `$LOAD_PATH` to list the directories that it searches when it needs to locate a required file. When `bundler/setup` removes those directories from `$LOAD_PATH`, Ruby can no longer find Gems.

To fix this, `bundler/setup` reads `Gemfile.lock`; for each Gem listed, it adds the directory that contains that Gem back to `$LOAD_PATH`. When finished, `require` only finds the proper versions of each Gem. This ensures that the specific Gem and version your app depends on is loaded, and not a conflicting version of that Gem.
```ruby
require 'bundler/setup'
```

To make sure everything works, we run the tests:
```terminal
$ ruby test/todolist_project_test.rb
Started with run options --seed 64477

  20/20: [=================================] 100% Time: 00:00:00, Time: 00:00:00

Finished in 0.00207s
20 tests, 36 assertions, 0 failures, 0 errors, 0 skips
```
If your tests don't work, make sure you're using the correct version of Ruby (2.5.0).

**Current project file structure**
```terminal
todolist_project
├── Gemfile
├── Gemfile.lock
├── README.md
├── lib
│   └── todolist_project.rb
└── test
    └── todolist_project_test.rb
```

### 4. Adding Another Gem
Added the use of `stamp` Gem into our code
```ruby
# lib/todolist_project.rb
require 'bundler/setup'
require 'stamp'              # Don't forget this line!!!!

class Todo
  def to_s # replaces original #to_s method
    result = "[#{done? ? DONE_MARKER : UNDONE_MARKER}] #{title}"
    result += due_date.stamp(' (Due: Friday January 6)') if due_date
    result
  end
end
```

```ruby
# test/todolist_project_test.rb
class TodoListTest < MiniTest::Test
  def test_to_s_with_due_date
    @todo2.due_date = Date.civil(2017, 4, 15)
    output = <<-OUTPUT.chomp.gsub(/^\s+/, '')
    ---- Today's Todos ----
    [ ] Buy milk
    [ ] Clean room (Due: Saturday April 15)
    [ ] Go to gym
    OUTPUT

    assert_equal(output, @list.to_s)
  end
end
```

Causes a LoadError since `stamp` gem is not available on our system
```terminal
$ ruby test/todolist_project_test.rb
.../todolist_project.rb:3:in `require': cannot load such file -- stamp (LoadError)
```

To fix that, we need to include this gem in our Gemfile
```ruby
# ... rest of file omitted

gem 'minitest-reporters', '~> 1.1'
gem 'stamp', '~> 0.6'
```

Then run `bundle install` again to update the `Gemfile.lock` and install the gem.
```terminal
$ ruby test/todolist_project_test.rb
Started with run options --seed 49287

  23/23: [=================================] 100% Time: 00:00:00, Time: 00:00:00

Finished in 0.00386s
23 tests, 39 assertions, 0 failures, 0 errors, 0 skips
```

### 5. Setting Up The Rakefile
We will create a `Rakefile` in the project's top-level directory to house `hello` and `test` tasks. `sh` is a Rake method to run the command `ruby ./test/todolist_project_test.rb`
```ruby
desc 'Say hello'
task :hello do
  puts "Hello there. This is the 'hello' task."
end

desc 'Run tests'
task :test do
  sh 'ruby ./test/todolist_project_test.rb'
end
```

Before running the `Rakefile` that, we need to add `rake` to our Gemfile.
```ruby
gem 'rake'
```

Then run `bundle install` again:
```terminal
bundle install
```

Make sure Rake recognizes the task
```terminal
$ bundle exec rake -T
rake hello  # Say hello
rake test   # Run tests
```
There are four main things to note here:
- We used `bundle exec rake` instead of just `rake`; you should use `bundle exec rake` whenever you use Rake with a project that uses Bundler.
- `bundle exec rake -T` displays a list of all tasks associated with your `Rakefile`. Here, we have the `:hello` and `:test` tasks that we added to our `Rakefile`. Note that it prints the task names without the colon (`:`).
- The command displays all the known tasks with their descriptions from the `desc` statement. 
- Although you use `bundle exec rake` to run the `rake` command, Rake itself is not a component of Bundler. We use `bundle exec` just to make sure we're using the Bundler environment with any code we run from `Rakefile`.


We can then run the tasks. Running `hello` task:
```terminal
$ bundle exec rake hello
Hello there. This is the 'hello' task.
```

Running the test file
```terminal
$ bundle exec rake test
ruby ./test/todolist_project_test.rb
Started with run options --seed 33301

  23/23: [=================================] 100% Time: 00:00:00, Time: 00:00:00

Finished in 0.00386s
23 tests, 39 assertions, 0 failures, 0 errors, 0 skips
```

Create a default task for most commonly run task.
```ruby
desc 'Run tests'
task :default => :test
```

We will now see `rake default` in the output of `bundle exec rake -T`, and when we run `rake` with no parameters, we're see our tests run:
```terminal
$ bundle exec rake
ruby ./test/todolist_project_test.rb
Started with run options --seed 12345

  23/23: [=================================] 100% Time: 00:00:00, Time: 00:00:00

Finished in 0.00386s
23 tests, 39 assertions, 0 failures, 0 errors, 0 skips
```

To allow use to add more test files in the project later and not need to update the Rakefile with each addition, we will remove the old test task and tell `rake/testtask` to build the list of tests for us.
```ruby
require "rake/testtask"

desc 'Say hello'
task :hello do
  puts "Hello there. This is the 'hello' task."
end

desc 'Run tests'
task :default => :test

Rake::TestTask.new(:test) do |t|
  t.libs << "test"
  t.libs << "lib"
  t.test_files = FileList['test/**/*_test.rb']
end
```
The code in the `Rake::TestTask.new` block tells `rake/testtask` that our tests and source code reside in the `test` and `lib` directories, and that all our test files reside in the `test` directory, and have a name that ends with `_test.rb`. When we run `bundle exec rake test` now, `rake/testtask` will load and run all the `*_test.rb` files it can find in the `test` directory or any of its subdirectories. Now we just have to add new test files whenever we need them.

Testing to ensure everything still works:
```terminal
$ bundle exec rake -T
rake default  # Run tests
rake hello    # Say hello
rake test     # Run tests

$ bundle exec rake test
Started with run options --seed 20139

  23/23: [=================================] 100% Time: 00:00:00, Time: 00:00:00

Finished in 0.00229s
23 tests, 39 assertions, 0 failures, 0 errors, 0 skips
```

**Current project file structure**
```terminal
todolist_project
├── Gemfile
├── Gemfile.lock
├── README.md
├── Rakefile
├── lib
│   └── todolist_project.rb
└── test
    └── todolist_project_test.rb
```

###  6. Preparing A Rubygem
Most Ruby projects use Rubygems as the distribution mechanism. This requires that we observe certain practices when building our project. Specifically, we must use a common directory structure and supply a `.gemspec` file. Most Gems also include both a `Gemfile` and a `Rakefile`, but don't require them.

When we're ready to prepare our project for distribution, we should investigate the requirements for Rubygems. We can find useful information in the [Rubygems documentation](http://guides.rubygems.org); it provides the details we're need to finish preparing our project as a Rubygem. The steps required include:
- Read the documentation.
- Prepare any additional directories that we need.
- Prepare a `README.md` file.
- Write documentation if necessary.
- Prepare our `.gemspec` file. Note that the actual `.gemspec` file has a name like `project.gemspec`, where "project" is our project name. See below for an example `.gemspec`.
- Add a `gemspec` statement to our `Gemfile`.

First we add a gemspec in our `Gemfile`
```ruby
source 'https://rubygems.org'

ruby '2.5.0'

gem 'minitest', '~> 5.10'
gem 'minitest-reporters', '~> 1.1'
gem 'stamp', '~> 0.6'
gem 'rake'

gemspec
```

Then we create a `todolist_project.gemspec` in the top level of our project directory.
```ruby
Gem::Specification.new do |s|
  s.name        = 'todolist_project'
  s.version     = '1.0.0'
  s.summary     = 'Todo List Manager!'
  s.description = 'This is a simple todo list manager!'
  s.authors     = ['Pete Williams']
  s.email       = 'pw@example.com'
  s.homepage    = 'http://example.com/todolist_project'
  s.files       = ['lib/todolist_project.rb', 'test/todolist_project_test.rb']
end
```

Then we add the following statement near the top of our `Rakefile`:

```ruby
require "bundler/gem_tasks"
```

The `bundler/gem_tasks` require file adds several tasks to our Rakefile that are common to Rubygems. Specifically, it defines these tasks (don't forget to use `bundle exec`):
- `rake build`: Constructs a `.gem` file in the `pkg` directory. This file contains the actual Rubygem that we will distribute.
- `rake install`: runs `rake build` then installs the program in our Ruby's Gem directory. This way, we can test the Gem without having to load information from our project directory.
- `rake release`: Send our `.gem` file to the remote Rubygems library for the world to download.

`bundler/gem_tasks` provides several additional tasks, but `build`, `install`, and `release` are the most important.

**Final project file structure**
```terminal
todolist_project
├── todolist_project.gemspec
├── Gemfile
├── Gemfile.lock
├── README.md
├── Rakefile
├── lib
│   └── todolist_project.rb
└── test
    └── todolist_project_test.rb
```

[See project in repository](https://github.com/cklim83/todolist_project)

[Back to Top](#section-links)
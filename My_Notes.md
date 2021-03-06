# My Notes

[Ruby](#ruby)
[Rspec](#rspec)
- [Rspec subject](#subject)

## Ruby
- Array methods in ruby
```ruby
  arr = ['a', 'b', 'c', 'd', 'e', 'f']
  arr.fetch(100)        #=> raise Exception
  arr.fetch(100,"oops") #=> "oops"
  
  arr.take(3)   #=> ['a', 'b', 'c']
  arr.drop(3)   #=> ['d', 'e', 'f']
  
  arr << 'g'             #=> ['a', 'b', 'c', 'd', 'e', 'f', 'g']
  arr.unshift('z')       #=> ['z', 'a', 'b', 'c', 'd', 'e', 'f', 'g']
  arr.insert(3, 'apple') #=> ['z', 'a', 'b', 'apple, 'c', 'd', 'e', 'f', 'g']
  arr.shift              #=> ['a', 'b', 'apple, 'c', 'd', 'e', 'f', 'g']
  arr.pop                #=> ['a', 'b', 'apple, 'c', 'd', 'e', 'f']
  arr.delete('apple')    #=> ['a', 'b', 'c', 'd', 'e', 'f']
  arr.unshift(nil).insert(3,nil).push(nil) #=> [nil ,'a', 'b', nil, 'c', 'd', 'e', 'f', nil]
  arr.compact            #=> ['a', 'b', 'c', 'd', 'e', 'f']
  arr                    #=> [nil ,'a', 'b', nil, 'c', 'd', 'e', 'f', nil]
  arr.compact!           # arr => ['a', 'b', 'c', 'd', 'e', 'f']
  arr.shift('a')         #=> ['a', 'a', 'b', 'c', 'd', 'e', 'f']
  arr.uniq               #=> ['a', 'b', 'c', 'd', 'e', 'f']
  
  arr.drop_while
  
```
- Hash method:
```ruby
.fetch
```

- when push an Array/Hash into an Array, you're actually pushing the address of the object
## Rails
- Joining names
```ruby
def name
  [firstname, lastname],join(' ')
end
```
- `includes` vs `joins`
  - if need association data, use `includes` 
  - else, use `joins` as it lazy loads data
- ActiveSupport::Inflector for some string hacking :)
- when using `has_one` association, doing `create_association` twice will cause one `INSERT INTO` and one `UPDATE` to happen, calling `association` will only bring up the first object, **should avoid creation if created**

## R-Spec
### Subject
- take `subject` like a Class as opposed to `let` as a instance variable
  - Implicit defined `subject`
  - if the argument of most outer example is a Class, an instance of that Class can be exposed to the inner examples with `.subject`
  ```ruby
    # under top level group
    describe Array do
      it "should be empty when first created" do
        subject.should be_empty  # similiar to array.should be_empty
      end
    end
    
    # under nested group
    describe Array do
      describe "when first created" do
        it "should be empty" do
          subject.should be_empty
        end
      end
    end
    
    # top most group wins (even there are two classes)
    
    class ArrayWithOneElement < Array
      def initialize(*)
        super
        unshift "first element"
      end
    end
  
    describe Array do
      describe ArrayWithOneElement do
        context "referenced as subject" do
          it "should be empty (because it is the Array declared at the top)" do
            subject.should be_empty
          end
        end
    
        context "created in the example" do
          it "should not be empty" do
            ArrayWithOneElement.new.should_not be_empty
          end
        end
      end
    end
    
  ```
  - Explicit `subject`
  ```ruby
  # subject in *top* level
  describe Array, "with some elements" do
    subject { [1,2,3] }
    it "should have the prescribed elements" do
      subject.should == [1,2,3]
    end
  end
  
  # subject in *nested* group
  describe Array do
    subject { [1,2,3] }
    describe "with some elements" do
      it "should have the prescribed elements" do
        subject.should == [1,2,3]
      end
    end
  end
  
  # access subject from *before block* (before method is not lazy loaded)
  describe Array, "with some elements" do
    subject { [] }
    before { subject.push(1,2,3) }
    it "should have the prescribed elements" do
      subject.should == [1,2,3]
    end
  end
  
  # invoke helper method from subject block
  describe Array do
    def prepared_array; [1,2,3] end
    subject { prepared_array }        # method is inside subject block, calling subject block will invoke method
    describe "with some elements" do
      it "should have the prescribed elements" do
        subject.should == [1,2,3]
      end
    end
  end
  
  # subject block is invoked at most once per example
  ```
### Hooks
- use `before(:each)` and avoid using `before(:all)`
  - `before(:each)` does not share state across examples, while `before(:all)` does
- `before/after(:each)` hooks are wrapped by the `around` hook but NOT `:all`

### Let
- `let`creates a method for your method?(lol)
```ruby
let(:foo) { Foo.new }

# is very nearly equivalent to this:
def foo
  @foo ||= Foo.new
end

describe '#type_id' do
  let(:resource) { FactoryGirl.create :device }
  let(:type)     { Type.find resource.type_id }

  it 'sets the type_id field' do
    expect(resource.type_id).to equal(type.id)
  end
end
```
- always check for false positive by intentionally breaking things

##Pundit
- when using policy(something) on UserPolicy, is equivalent to UserPolicy.new(current_user, something)

## Gemlist
- Stripe  -> membership
- Recurly -> subscription
- Clearance, Devise -> authentication
- Pundit -> authorization
- Rspec -> testing
- Capybara -> feature testing gem
- Slim-rails -> use Slim format
- Filterrific -> data filter bar
- Searchkick -> search bar
- Puma -> server
- Sidekiq -> jobs
- Redis -> store jobs
- Carrierwave -> uploading
- Fog -> onliine storage
- Guard -> auto-testing
- Figaro -> application.yml gitignore
- Sendgrid -> email
- Will_paginate -> pagination
- Better_errors -> error interface
- bootstrap_rails / foundation_rails -> front-end framework
- Spring -> rails app preloader
- Meta_request
- Binding_of_caller
- Awesome_print
- IFTTT / zapier -> app 
- Cocoon -> nested_attribute form gem
- jquery-turbolinks -> gem to solve turbolink problems on jquery

# define_method 
# elasticsearch
- Commands:
  - console      Launch in the current console.
  - start        Start in the background as a daemon process.
  - stop         Stop if running as a daemon or in another console.
  - restart      Stop if running and then start.
  - condrestart  Restart only if already running.
  - status       Query the current status.
  - install      Install to start automatically when system boots.
  - remove       Uninstall.
  - dump         Request a Java thread dump if running.
## Binstubs
- Prepares the environment before dispatching the call to the original executable.
- `bundle exec <command>` is so essential. It ensures the right versions of dependencies get activated, ensuring a consistent ruby runtime environment. 





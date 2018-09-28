### Mutations
---
https://github.com/cypriss/mutations



```sh
gem install mutations
gem 'mutations'
```

```ruby
class UserSignup < Mutations::Command
  required do
    stirng :email, matches: EMAIL_REGEX
    string :name
  end
  optoinal do
    boolean :newsletter_subscribe
  end
  def execute
    user = User.create!(input)
    NewsletterSubscriptions.create(email: email, user_id: user.id) if newsletter_subscribe
    UserMailer.async(:deliver_welcome, user.id)
    user
  end
end
def create
  outcome = UserSignup.run(params[:user])
  if outcome.success?
    render json: {message: "Great success, #{outcome.result.name}!"}
  else
    render json: outcome.errors.symbolic, status: 422
  end
end

```

```
app/mutations
app/mutations/users/sinup.rb
app/mutations/users/login.rb
app/mutations/users/update_profile.rb
app/mutatoins/users/change_password.rb
...
app/mutations/articles/create.rb
app/mutations/articles/update.rb
app/mutations/articles/publish.rb
app/mutations/articles/comment.rb
...
app/mutations/ideas/upsert.rb
...
```

```ruby
outcome = UserSignup.run(params)
if outcome.success?
  user = outcome.result
else
  render outcome.errors
end

user = UserSignup.run!(params)
# returns the results of execute, raises Mutations::ValidationException

class CreateComment < Mutations::Command
  required do
    model :user
    model :article
    string :comment, max_length: 500
  end
  def execute; ...; end
end
def somewhere
  outcome = CreateComment.run(params[:comment],
    user: current_user,
    article: Article.find(params[:article_id])
  )
end


class YourMutation < Mutations::Command
end

required do
  string :name, max_length: 10
  string :state, in: %w(AL AK AR ... WY)
  integer :age
  boolean :is_special, default: true
  model :account
end

optional do
  array :tags, class: String
  hash :prefs do
    boolean :smoking
    boolean:view
  end
end


def execute
  record = do_thing(input)
  record
end



self.inputs
self.email
self.email=(val)
self.email_present?

if email =~ /aol.com/
  add_error(:email, :old_school, "Wow you still use AOL?")
  return
end


def execute
  "WIN!"
end
outcome = YourMutation.run(...)
outcome.result # => "WIN!"


outcome = UserSignup.run(name: "Bob", newsletter_subscribe: "Wat")
unless outcome.success?
  outcome.errors.symbolic # => {email: :required, newsletter_subscribe: :boolean}
  outcome.errrors.message # => {email: "Email is reuired", newsletter_subscribe: "Newsletter Subscription isn't a boolean"}
  outcome.errors.message_list # => ["Email is required", "Newsletter Subscription isn't a boolean"]
end


def validate
  if password != password_confirmation
    add_error(:password_confirmation, :doesnt_match, "Your passwords don't match")
  end
end
outcome.errors.symbolic # => {password_confirmation: doesnt_match}
outcome.errors.message # => {password_confirmation: "Your passwords don't match"}



def execute
  if password != password_confirmation
    add_error(:password_confirmation, :doesnt_match, "Your passwords don't match")
    return
  end
end
outcome.errors.symbolic # => {password_confirmation: :doesnt_match}
outcome.errors.message # => {password_confirmation: "Your password don't match"}

```




# Adding Some Authentication with Ember Simple Auth

So out School Directory App is pretty decent, but it really needs one more feature. We need to restrict edit, create, and delete functionality to logged in users. We'll assume that anyone can view our directory, and anyone with a login is an admin with CRUD access. We had to add authentication to our Rails api server and we will use that to house our authentication. We are going to use the Devise Rails library and we did a little customization there. You can clone and inspect those changes in the repo which I will link to in the notes.


Start by creating a login route and Controller

ember g route login && ember g controller login

set up the login template: Taken directly from the Ember Simple Auth docs:

```html
<form {{action 'authenticate' on='submit'}}>
  <label for="identification">Login</label>
  {{input id='identification' placeholder='Enter Login' value=identification}}
  <label for="password">Password</label>
  {{input id='password' placeholder='Enter Password' type='password' value=password}}
  <button type="submit">Login</button>
  {{#if errorMessage}}
    <p>{{errorMessage}}</p>
  {{/if}}
</form>
```

Setup the login controller:

```JavaScript
session: Ember.inject.service('session'),

actions: {
  authenticate() {
    let { identification, password } = this.getProperties('identification', 'password');
    this.get('session').authenticate('authenticator:devise', identification, password).catch((reason) => {
      this.set('errorMessage', reason.error || reason);
    });
  }
}
```

Setup our devise authenticator in `app/authenticators/devise.js`

```JavaScript
import DeviseAuthenticator from 'ember-simple-auth/authenticators/devise';

export default DeviseAuthenticator.extend({
  serverTokenEndpoint: 'http://localhost:3000/users/sign_in'
});
```


## Links

http://blog.andrewray.me/how-to-set-up-devise-ajax-authentication-with-rails-4-0/
https://www.simplify.ba/articles/2016/06/18/creating-rails5-api-only-application-following-jsonapi-specification/

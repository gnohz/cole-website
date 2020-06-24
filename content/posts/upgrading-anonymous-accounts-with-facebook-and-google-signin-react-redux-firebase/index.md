---
title: "Upgrading Anonymous Accounts With Facebook and Google Signin - React Redux Firebase"
date: 2020-06-23T23:45:20-04:00
draft: false
tags: ["firebase", "authentication"]
categories: ["Technical"]
---

# The Goal

It's easy enough to get facebook and google signin working with react-redux-firebase. What isn't as well supported, is upgrading anonymous accounts through facebook and google signin.

Here is what we'll be building ([Prateek](https://prateek.humane) built this ui)

{{< image src="./signinModal.png" caption="Signin Modal" >}}

# Preparation

## Accounts and stuff

Activate google and facebook login in the Firebase console. You'll need to create a Facebook developer account as well and activate Facebook login. Make sure to add your client id and secret to Firebase console.

## Dependencies

We will be using `react-google-signin` and `react-facebook-signin`. They help to abstract away some of the boilerplate code.

# Coding

## Google Signin

`react-google-signin` provides a ui element, but we weren't especially happy with it and opted for rendering our own.

{{< highlight javascript >}}
<GoogleLogin
  clientId="Your Google clientId"
  render={renderProps => (
    <Button
      variant="contained"
      color="inherit"
      startIcon={
        <img
          style={{ height: "30px" }}
          src={google_icon}
          alt=""
        />
      }
      size="small"
      fullWidth
      onClick={renderProps.onClick}
      style={{
        height: "100%"
      }}
    >
      Sign in with Google
    </Button>
  )}
  buttonText="Login"
  onSuccess={this.googleSignin}
  onFailure={this.googleSigninFailure}
  isSignedIn={true}
  cookiePolicy={'single_host_origin'}
/>
{{</ highlight >}}

Now is where the "onSuccess" callback becomes important. We want to make sure that it successfully links the current anonymous user with the account that is signing in. We take care of this as follows:

{{< highlight javascript >}}
googleSignin = (googleUser) => {
  const credential = this.props.firebase.auth.GoogleAuthProvider.credential(
    googleUser.getAuthResponse().id_token);

  this.props.firebase.auth().currentUser.linkWithCredential(credential)
    .then(function(usercred) {
      var user = usercred.user;
      console.log("Anonymous account successfully upgraded", user);
    }).catch(function(error) {
      console.log("Error upgrading anonymous account", error);
    });

  this.props.firebase.login({
    credential
  });

  this.props.handleClose();
};
{{</ highlight >}}

The idea is that we use the token returned by the google signin to create a firebase credential, and then link it to the anonymous user. This basically creates the new user with the same uid as the anonymous user.

Finally we login with the credential inorder to set the local state correctly.

## Facebook

We approach facebook login very similarly.

{{< highlight javascript >}}
<FacebookLogin
  /* onClick={componentClicked} */
  appId={Your Facebook appId}
  callback={this.facebookSignin}
  onFailure={this.facebookSigninFailure}
  responseType="token"
  render={renderProps => (
    <Button
      variant="contained"
      color="primary"
      size="small"
      startIcon={
        <img
          style={{ height: "30px" }}
          src={facebook_icon}
          alt=""
        />
      }
      fullWidth
      onClick={renderProps.onClick}
      /* disabled={renderProps.disabled} */
    >
      Sign in with Facebook
    </Button>
  )}
/>
{{</ highlight >}}

And facebookSignin. Notice that I am using event.accessToken instead of event.authResponse.accessToken like in the firebase docs because the response type that react-facebook-login passes to the event handler is not the same as that of the approach the firebase docs assume that you will be using.

{{< highlight javascript >}}
facebookSignin = (event) => {
  console.log(event);
  console.log(event.authResponse);
  const credential = this.props.firebase.auth.FacebookAuthProvider.credential(
    event.accessToken);

  this.props.firebase.auth().currentUser.linkWithCredential(credential)
    .then(function(usercred) {
      var user = usercred.user;
      console.log("Anonymous account successfully upgraded", user);
    }).catch(function(error) {
      console.log("Error upgrading anonymous account", error);
    });

  this.props.firebase.login({
    credential
  });

  this.props.handleClose();
};
{{</ highlight >}}


# Conclusion

Hopefully this helps someone out who goes through the same process I did. Let me know if you have any questions!


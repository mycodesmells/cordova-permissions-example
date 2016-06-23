# Permissions Management in Android 6

If you work with mobile or hybrid applications, you might have already heard about some changes regarding permissions management in Android 6. But how they affect you as a developer? Do you need to worry about them? In my case those changes broke the app, so read on if you don't want to be surprised at some point with yours.

### What has changed?

- old installation
    - permissions defined ahead
    - all-or-none approach
    - nothing do to in the app
- new installation
    - no permissions
    - asking for them within app one-by-one
    - user may pick and choose permissions
    - user may be asked numerous times if disagrees
    - having to handle rejection

### Example

Application code:

    code

Android 4.3 installation:

    screens 43 install

Android 4.3 runtime:

    screenshot

Android 6.0 installation:

    screens

Android 6.0 runtime:

    screens

### Consequences

- not much for hybrid code
- has to be handled by Java plugin [link to docs]():

    code example

### Summary

- what happenned in my case
- make sure that plugin supports new approach

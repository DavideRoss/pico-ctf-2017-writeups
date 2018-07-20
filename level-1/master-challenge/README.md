# Master Challenge
## Level 1 - 50 points

### Description
> I really need to login to this [website](http://shell2017.picoctf.com:43393/), but the developer hasn't implemented login yet. Can you help?

### Hints
> * Where does the password check actually occur?
> * Can you interact with the javascript directly?

### Solution

The vulnerability on this challenge is not a real vulnerability, instead is the (really) bad practice to perform form checking on frontend, where we can change the code in runtime to do nasty things.

Open the Inspector (I use Chrome), in the "Source" tab search for `client.js` file (hitting CMD/CTRL+P or searching on the left sidebar). Now you can edit the JS file locally at runtime. You have just to change the `validate` function:

```js
//Validate the password. TBD!
function validate(pword){
  //TODO: Implement me
  return true;
}
```

Save the file hitting CMD/CTRL+S and (without reloading the page) hit the "Submit" button. The flag wil be appended after the form.

### Flag
```
client_side_is_the_dark_sidea99c64effed2c2f1c9347eff536e949c
```